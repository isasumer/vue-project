<template>
  <div class="container jumbotron bg-info">
    <h1 class="display-4">Hello, Hızlı Yazma Yarışmasına Hoş Geldiniz!!</h1>
    <p class="lead">
      Hızını test et
    </p>
    <hr class="my-4" />
    <div v-if="isFinish" class="alert alert-primary">
      <h3>
        Oyun Bitti
      </h3>
      <p class="display-4">{{ yks }} Yazılan kelime</p>
      <div>Doğruluk Yüzdesi: % {{ truePercent }}</div>
      <div>Doğru Sayısı: {{ trueCount }}</div>
      <div>Yanlış Sayısı: {{ falseCount }}</div>
    </div>
    <div v-else>
      <div class="card">
        <div class="card-body">
          <span
            v-for="(word, key) in words.filter((data, index) => index < 15)"
            :key="key"
            v-bind:class="key != 0 || writingWordControl"
            class="words"
            >{{ word }}</span
          >
        </div>
      </div>
      <div class="card">
        <div class="card-body">
          <div class="input-group input-group-lg">
            <input type="text" class="form-control" v-model="writingWord" />
            <div class="input-group-append input-group-lg">
              <button class="btn btn-outline-secondary" disabled type="button">
                {{ timer }} sn
              </button>
              <button
                :disabled="isRunning"
                class="btn btn-outline-secondary"
                type="button"
                @click="getWords"
              >
                Yenile
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import data from "../assets/data.json";
export default {
  data() {
    return {
      words: [],
      writingWord: null,
      isTrue: true,
      trueCount: 0,
      falseCount: 0,
      timer: 10,
      interval: false,
      isRunning: false,
      isFinish: false,
      wordList: data
    };
  },

  watch: {
    writingWord(val) {
      if (!val || val === " ") {
        this.writingWord = "";
        return;
      }
      !this.isRunning && this.toggleTimer();
      const word = this.words[0].slice(0, val.length);
      const userWord = val.replace(" ", "");
      this.isTrue = word === userWord;

      if (val.indexOf(" ") !== -1) {
        this.isTrue ? this.trueCount++ : this.falseCount++;
        this.words.splice(0, 1);
        this.writingWord = "";
        this.isTrue = true;
      }
    }
  },

  computed: {
    writingWordControl() {
      return this.isTrue ? "first-word" : "first-word bg-danger";
    },
    yks() {
      const yks = 300 - this.words.length;
      return yks;
    },
    truePercent() {
      const percent = (this.trueCount / this.yks) * 100;
      return percent;
    }
  },
  mounted() {
    this.getWords();
  },

  methods: {
    getWords() {
      this.words = this.wordList.sort(() => Math.random() - 0.5).splice(0, 300);
    },
    toggleTimer() {
      this.isRunning = true;
      this.interval = setInterval(this.timeProcess, 1000);
    },
    timeProcess() {
      this.timer--;
      if (this.timer === 0) {
        clearInterval(this.interval);
        this.isFinish = true;
      }
    }
  }
};
</script>

<style>
.words {
  margin-left: 10px;
  font-size: 30px;
  font-weight: 500;
  padding: 10px;
}
.first-word {
  background-color: rgba(153, 153, 153, 0.404);
  border-radius: 10px;
}
</style>

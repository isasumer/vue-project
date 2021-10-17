<template>
  <div class="container jumbotron bg-info">
    <h1 class="display-4">Hello, Hızlı Yazma Yarışmasına Hoş Geldiniz!!</h1>
    <p class="lead">
      Hızını test et
    </p>
    <div>Doğru Sayısı: {{ trueCount }}</div>
    <div>Yanlış Sayısı: {{ falseCount }}</div>
    <hr class="my-4" />
    <div v-if="isFinish" class="alert alert-primary">
      <h1>
        Oyun Bitti
      </h1>
    </div>
    <div v-else>
      <div class="card">
        <div class="card-body">
          <span
            v-for="(word, key) in words"
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
              <button class="btn btn-outline-secondary" type="button">
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
export default {
  data() {
    return {
      words: ["isa", "sumer", "vscode", "msi", "vue.js"],
      writingWord: null,
      isTrue: true,
      trueCount: 0,
      falseCount: 0,
      timer: 5,
      interval: false,
      isRunning: false,
      isFinish: false
    };
  },

  watch: {
    writingWord(val) {
      !this.isRunning && this.toggleTimer();
      const word = this.words[0].slice(0, val.length);
      const userWord = val.replace(" ", "");
      this.isTrue = word === userWord;

      if (val.indexOf(" ") !== -1) {
        this.isTrue ? this.trueCount++ : this.falseCount++;
        this.words.splice(0, 1);
        this.writingWord = "";
      }
    }
  },
  computed: {
    writingWordControl() {
      return this.isTrue ? "first-word" : "first-word bg-danger";
    }
  },

  methods: {
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

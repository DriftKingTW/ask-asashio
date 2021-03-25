<template>
  <div>
    <h1>Ask me something...</h1>
    <div style="margin-bottom: 30px">
      <img
        :src="require('./assets/' + img)"
        :class="{ loading: isLoading }"
        style="width: 200px; min-width: 200px"
        alt=""
      />
    </div>
    <br />
    <input type="text" v-model="question" />
    <p v-text="answer ? answer + '...' : ''"></p>
    <p>
      Powered by
      <a href="https://yesno.wtf/" target="_blank">https://yesno.wtf/</a>
    </p>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      question: "",
      answer: "",
      isLoading: true,
    };
  },
  methods: {
    yesNo() {
      this.answer = "thinking";
      fetch("https://yesno.wtf/api")
        .then((res) => (res.ok ? res : console.log("failed")))
        .then((res) => res.json())
        .then((data) => {
          console.log(data.answer);
          this.answer = data.answer;
          this.isLoading = false;
        })
        .catch((err) => console.log(err));
    },
  },
  computed: {
    img() {
      if (this.answer === "yes") {
        return "Asashio_yes.png";
      } else if (this.answer === "no") {
        return "Asashio_no.png";
      } else if (this.answer === "maybe") {
        return "Asashio_maybe.png";
      } else {
        return "loading.png";
      }
    },
  },
  watch: {
    question(newQuestion) {
      if (newQuestion.indexOf("?") > -1 || newQuestion.indexOf("？") > -1) {
        this.yesNo();
      } else {
        this.answer = "There's usually a question mark behind the question...";
        this.isLoading = true;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.loading {
  animation: rotation 2s infinite linear;
}

@keyframes rotation {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(359deg);
  }
}
</style>

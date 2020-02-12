<template>
  <div id="app">
    <img alt="Quiz-app logo" class="logo" src="./assets/logo.png" />
    <Header :correctAnswer="correctAnswer" :falseAnswer="falseAnswer" />
    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <question-box
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
            :buttonNextHidden="buttonNextHidden"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
export default {
  name: "App",
  components: {
    Header,
    QuestionBox: QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      correctAnswer: 0,
      falseAnswer: 0,
      buttonNextHidden: false
    };
  },
  methods: {
    next() {
      if (this.index < 9) {
        this.index++;
      } else {
        this.buttonNextHidden = true;
      }
    },

    increment(isCorrect) {
      if (isCorrect) {
        this.correctAnswer++;
      } else {
        this.falseAnswer++;
      }
    }
  },
  mounted() {
    axios
      .get("https://opentdb.com/api.php?amount=10&type=multiple")
      .then(res => (this.questions = res.data.results));
  }
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

.logo {
  width: 150px;
  height: 150px;
}
</style>

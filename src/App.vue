<template>
  <div id="app">
    <b-container>
      <b-navbar variant="faded">
        <b-navbar-brand>
          <img alt="Quiz-app logo" class="logo" src="./assets/logo.png" />
          GKQuiz!?
        </b-navbar-brand>
      </b-navbar>

      <Header :correctAnswer="correctAnswer" :falseAnswer="falseAnswer" />

      <b-row>
        <b-col sm="6" offset="3" v-if="!inGame">
          <question-box
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
        <b-col v-else sm="8" offset="2">
          <b-table class="mt-5" stacked :items="resumeAnswer"></b-table>
          <b-button class="newGame" @click="newGame">New Game</b-button>
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
      resumeAnswer: [],
      index: 0,
      correctAnswer: 0,
      falseAnswer: 0,
      inGame: false
    };
  },
  methods: {
    next(ownAnswer, correct) {
      if (this.index < 9) {
        this.index++;
        const answers = {
          question: this.questions[this.index].question,
          yourAnswer: ownAnswer,
          correctAnswer: correct
        };
        this.resumeAnswer.push(answers);
      } else {
        this.inGame = true;
      }
    },

    increment(isCorrect) {
      if (isCorrect) {
        this.correctAnswer++;
      } else {
        this.falseAnswer++;
      }
    },
    newGame() {
      this.questions = [];
      this.resumeAnswer = [];
      this.index = 0;
      this.correctAnswer = 0;
      this.falseAnswer = 0;
      this.inGame = false;
      axios
        .get("https://opentdb.com/api.php?amount=10&type=multiple")
        .then(res => (this.questions = res.data.results));
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
@import url("https://fonts.googleapis.com/css?family=Autour+One&display=swap");
@import url("https://fonts.googleapis.com/css?family=Autour+One|Roboto:400,700,900&display=swap");

:root {
  --color-primary-red: #ff525d;
  --color-primary-yellow: #ffd15c;
  --color-primary-grey: #5c5e70;
  --color-secondary-grey: #5c5e7071;
  --color-primary-font: #f9eed7;
  --color-secondary-font: #e9ecef;
}
#app {
  font-family: "Roboto", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav > .navbar-brand {
  margin: 30px auto !important;
}

.navbar-brand {
  font-family: "Autour One", cursive, sans-serif;
  font-size: 50px;
  color: var(--color-primary-grey) !important;
}

.logo {
  width: 150px;
  height: 150px;
}

[data-label] {
  font-family: "Autour One", cursive, sans-serif;
}

[data-label] > div {
  font-family: "Roboto", sans-serif;
}

[data-label="Question"] {
  background-color: var(--color-primary-grey);
  color: var(--color-primary-font);
}

.newGame {
  text-transform: uppercase;
  font-weight: bold;
  color: var(--color-primary-font);
  margin: 50px 0;
  border-radius: 0;
  background-color: var(--color-primary-yellow);
  border-color: var(--color-primary-yellow);
}

.newGame:hover {
  background-color: var(--color-primary-red);
  border-color: var(--color-primary-red);
}
</style>

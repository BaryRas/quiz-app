<template>
  <div>
    <b-jumbotron class="jumbotron" v-if="currentQuestion">
      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          :class="answeredClass(index)"
          v-for="(answer, index) in suffleAnswers"
          :key="index"
          @click.prevent="selectedAnswer(index)"
          >{{ answer }}</b-list-group-item
        >
      </b-list-group>

      <b-button
        variant="primary"
        :disabled="selectedIndex === null || answered"
        @click="submitAnswer"
        >Submit</b-button
      >
      <b-button
        variant="success"
        @click.prevent="next"
        v-show="!buttonNextHidden"
        >Next</b-button
      >
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
    buttonNextHidden: Boolean
  },
  data() {
    return {
      selectedIndex: null,
      suffleAnswers: [],
      correctIndex: null,
      answered: false
    };
  },
  methods: {
    selectedAnswer(index) {
      this.selectedIndex = index;
      this.activeClass = true;
    },
    suffleAnswer() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.suffleAnswers = _.shuffle(answers);
      this.correctIndex = this.suffleAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    answeredClass(index) {
      let answerClass = "";

      if (!this.answered && this.selectedIndex === index) {
        answerClass = "selected";
      } else if (this.answered && this.correctIndex === index) {
        answerClass = "correct";
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== this.selectedIndex
      ) {
        answerClass = "incorrect";
      }

      return answerClass;
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.suffleAnswer();
      }
    }
  }
};
</script>

<style scoped>
.jumbotron {
  margin-top: 20px;
}
.list-group {
  margin-bottom: 20px;
}

.list-group-item:hover {
  background: #e9ecef;
  cursor: pointer;
}

.selected {
  background-color: #5c5e70;
  color: #e9ecef;
}

.correct {
  background-color: #218838;
}

.incorrect {
  background-color: #ffc107;
}

.btn {
  margin: 0 5px;
}
</style>

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
        class="submit"
        :disabled="selectedIndex === null || answered"
        @click="submitAnswer"
        >Submit</b-button
      >
      <b-button
        class="next"
        @click.prevent="
          next(suffleAnswers[selectedIndex], suffleAnswers[correctIndex])
        "
        v-show="answered"
        >Next</b-button
      >
    </b-jumbotron>
    <div class="spinner" v-else>
      <b-spinner
        style="width: 3rem; height: 3rem;"
        label="Large Spinner"
        type="grow"
      ></b-spinner>
      <b-spinner
        style="width: 3rem; height: 3rem;"
        label="Large Spinner"
        type="grow"
      ></b-spinner>
      <b-spinner
        style="width: 3rem; height: 3rem;"
        label="Large Spinner"
        type="grow"
      ></b-spinner>
    </div>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
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
  margin-top: 50px;
}

.spinner {
  margin-top: 250px;
}

.list-group {
  margin-bottom: 20px;
}

.list-group-item:hover {
  background: var(--color-secondary-font);
  cursor: pointer;
}

.selected {
  background-color: var(--color-primary-grey);
  color: var(--color-secondary-font);
}

.correct {
  background-color: var(--color-primary-yellow);
}

.incorrect {
  background-color: var(--color-primary-red);
}

.btn {
  margin: 0 5px;
}
.submit,
.next {
  text-transform: uppercase;
  font-weight: bold;
  color: var(--color-primary-font);
  border-radius: 0;
  background-color: var(--color-primary-grey);
  border-color: var(--color-primary-grey);
}

.submit:hover,
.next:hover {
  background-color: var(--color-secondary-grey);
  border-color: var(--color-secondary-grey);
}
</style>

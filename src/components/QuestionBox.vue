<template>
  <div>
    <b-jumbotron v-if="currentQuestion">
      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          :class="[selectedIndex === index ? 'selected' : '']"
          v-for="(answer, index) in answers"
          :key="index"
          @click.prevent="selectedAnswer(index)"
          >{{ answer }}</b-list-group-item
        >
      </b-list-group>

      <b-button variant="primary" :class="!activeClass ? 'disabled' : ''"
        >Submit</b-button
      >
      <b-button variant="success" @click.prevent="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function
  },
  data() {
    return {
      selectedIndex: null,
      activeClass: false,
      suffleAnswers: []
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
        this.suffleAnswer();
      }
    }
  }
};
</script>

<style scoped>
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
  background-color: #ff525d;
}

.btn {
  margin: 0 5px;
}
</style>

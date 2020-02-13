<template>
  <div>
    <b-container>
      <h3>
        <i class="fa fa-check-square"></i> : {{ correctAnswer }} |
        <i class="fa fa-times-circle"></i> :
        {{ falseAnswer }}
      </h3>
      <b-progress class="mt-2" :max="max" show-value>
        <b-progress-bar
          :value="correctAnswer"
          class="bg-correct"
        ></b-progress-bar>
        <b-progress-bar :value="falseAnswer" class="bg-false"></b-progress-bar>
      </b-progress>
    </b-container>
  </div>
</template>

<script>
export default {
  props: {
    correctAnswer: Number,
    falseAnswer: Number
  },
  data() {
    return {
      // value: 10,
      max: 10
    };
  }
};
</script>

<style scoped>
h3 {
  font-family: "Autour One", cursive, sans-serif;
  text-align: left;
  margin-top: 20px;
}
.fa-check-square {
  color: var(--color-primary-yellow);
}

.fa-times-circle {
  color: var(--color-primary-red);
}

.bg-correct {
  background-color: var(--color-primary-yellow);
}

.bg-false {
  background-color: var(--color-primary-red);
}
</style>

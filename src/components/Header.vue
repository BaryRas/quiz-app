<template>
  <div>
    <h3>Counter quiz :</h3>
    <b-progress class="mt-2" :max="max" show-value>
      <b-progress-bar :value="correctAnswer" variant="success"></b-progress-bar>
      <b-progress-bar :value="falseAnswer" variant="warning"></b-progress-bar>
    </b-progress>
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
  text-align: left;
  margin-top: 20px;
}
</style>

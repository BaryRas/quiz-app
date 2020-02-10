<template>
  <div id="app">
    <img alt="Quiz-app logo" class="logo" src="./assets/logo.png" />
    <Header></Header>
    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <question-box></question-box>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
export default {
  name: "App",
  components: {
    Header,
    QuestionBox: QuestionBox
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

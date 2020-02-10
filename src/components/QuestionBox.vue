<template>
  <div>
    <b-jumbotron>
      <template v-slot:header>BootstrapVue</template>

      <template v-slot:lead>
        Some questions here?!
      </template>

      <hr class="my-4" />

      <p>
        List of answers
      </p>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {};
</script>

<style></style>

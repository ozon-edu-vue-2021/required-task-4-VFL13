<template>
  <div id="app">
    <Form />
  </div>
</template>

<script>
import Form from "./components/Form.vue";

export default {
  name: "App",
  components: {
    Form,
  },
};
</script>

<style>
body {
  width: 100%;
  display: grid;
  justify-content: center;
  align-items: center;
  background: #ffffff;
  font-family: "roboto", sans-serif;
}
#app {
  max-width: 660px;
}
</style>

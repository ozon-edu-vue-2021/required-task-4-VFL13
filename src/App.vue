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
  display: flex;
  justify-content: center;
  align-items: center;
  background: #ffffff;
  font-family: "roboto", sans-serif;
}

/*#app {*/
/*  font-family: Avenir, Helvetica, Arial, sans-serif;*/
/*  color: #2c3e50;*/
/*  background-color: #fafafa;*/

/*  box-sizing: border-box;*/
/*}*/

#app {
  /*height: 100%;*/
}
</style>

<template>
  <v-app>
    <v-container class="container">
      <TodoForm></TodoForm>
    </v-container>
  </v-app>
</template>

<script>
// import TodoList from "./components/TodoList.vue";
import TodoForm from "./components/TodoForm.vue";

export default {
  name: "App",
  components: {
    TodoForm,
  },
};
</script>

<style>
body {
  margin: 0;
  text-align: center;
}

.container {
  max-width: 100% !important;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;

}
</style>

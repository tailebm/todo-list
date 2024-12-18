<template>
  <Header />
  <TodoList />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import TodoList from "./components/TodoList.vue";
import Header from "./components/Header.vue";

export default defineComponent({
  name: "App",
  components: {
    Header,
    TodoList,
  },
});
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
  width: 100px; /* Ajustez la taille */
  height: auto;
  margin-right: 10px;
}
.header {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
}
h1 {
  font-size: 2.5rem;
  color: #4caf50;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}
</style>

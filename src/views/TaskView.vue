<template>
  <div class="container">
    <Task />
  </div>
</template>

<script>
import Task from "../components/Task";

export default {
  name: "TaskView",

  components: {
    Task,
  },
};
</script>

<style scoped>
.container {
  background-color: rgb(32, 32, 32);
  min-height: 100vh;
  padding: 0;
  margin: 0;
}
</style>

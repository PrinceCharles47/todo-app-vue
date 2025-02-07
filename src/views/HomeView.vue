<template>
  <div class="container">
    <ToDo />
  </div>
</template>

<script>
import ToDo from "../components/ToDo";

export default {
  name: "Home",

  components: {
    ToDo,
  },
};
</script>

<style scoped>
.container{
  background-color: rgb(32, 32, 32);
  min-height: 100vh;
  padding: 0;
  margin: 0;
}
</style>
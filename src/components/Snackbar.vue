<template>
  <v-snackbar v-model="open" color="success">
    <p class="text-center">{{ message }}</p>
  </v-snackbar>
</template>

<script>
export default {
  name: "Snackbar",
  props: {
    open: Boolean,
    message: String,
  },
  data: () => ({}),
  methods: {
    closeSnackbar() {
      this.$emit("close");
    },
  },
  mounted() {
    window.setInterval(() => {
      if (open) {
        this.closeSnackbar();
      }
    }, 5000);
  },
  beforeDestroy() {
    this.closeSnackbar();
  },
};
</script>

<style scoped>
p {
  margin: 0;
}
</style>

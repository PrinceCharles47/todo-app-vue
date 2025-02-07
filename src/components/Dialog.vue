<template>
  <v-dialog v-model="open" persistent max-width="290" dark>
    <v-card class="pt-4">
      <v-card-text class="text-center">Are you sure you want to delete "{{ task }}" ?</v-card-text>
      <v-card-actions>
        <v-btn color="green darken-1" text @click="closeDialog"> Close </v-btn>
        <v-spacer></v-spacer>
        <v-btn color="red darken-1" text @click="agreeFn"> Agree </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "Dialog",
  props: {
    open: Boolean,
    task: String,
  },
  data: () => ({}),
  methods: {
    closeDialog() {
      return this.$emit("toggle");
    },
    agreeFn() {
      return this.$emit("agree");
    },
  },
};
</script>

<template>
  <div>
    <v-hover v-slot="{ hover }">
      <div class="todo-task">
        <v-checkbox
          v-model="val"
          @click="clickCheckbox"
          class="px-4 py-1"
          dark
        />
        <p :class="val ? 'todo-task-selected' : 'todo-task-default'">
          {{ task }}
        </p>

        <v-btn
          v-if="hover"
          class="ml-auto mr-3"
          fab
          plain
          dark
          small
          color="pink"
          @click="deleteTask"
        >
          <v-icon dark> mdi-close </v-icon>
        </v-btn>
      </div>
    </v-hover>
    <v-divider dark />
  </div>
</template>

<script>
export default {
  name: "TaskItem",

  props: {
    isComplete: Boolean,
    task: String,
  },
  data: () => ({
    val: false,
  }),
  methods: {
    clickCheckbox() {
      this.$emit("check");
    },
    deleteTask() {
      this.$emit("delete");
    },
  },
  computed: {},
  mounted() {
    this.val = this.isComplete;
  },
};
</script>

<style scoped>
.todo-task {
  width: 100%;
  display: flex;
  align-items: center;
}

.todo-task-default {
  /*color: white;*/
  margin-bottom: 5px;
}

.todo-task-selected {
  text-decoration: line-through;
  margin-bottom: 5px;
  color: rgb(161, 161, 161);
}
</style>

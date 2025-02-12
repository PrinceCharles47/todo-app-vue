<template>
  <div>
    <v-hover v-slot="{ hover }">
      <div class="todo-task">
        <v-checkbox
          v-model="task.isComplete"
          @click="clickCheckbox"
          class="px-4 py-1"
          dark
        />
        <p
          :class="task.isComplete ? 'todo-task-selected' : 'todo-task-default'"
        >
          {{ task.task }}
        </p>

        <div class="ml-auto">
          <v-btn
            v-if="hover"
            class="ml-auto"
            fab
            plain
            dark
            small
            @click="redirect('task', task.id)"
          >
            <v-icon dark> mdi-eye-outline </v-icon>
          </v-btn>
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
      </div>
    </v-hover>
    <v-divider dark />
  </div>
</template>

<script>
export default {
  name: "TaskItem",

  props: {
    taskDetails: Object,
  },
  data: () => ({
    task: {},
  }),
  methods: {
    redirect(routeName, taskId) {
      return this.$router.push({
        name: routeName,
        params: { id: taskId },
      });
    },
    clickCheckbox() {
      this.$emit("check");
    },
    deleteTask() {
      this.$emit("delete");
    },
  },
  computed: {},
  mounted() {
    this.task = this.taskDetails;
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

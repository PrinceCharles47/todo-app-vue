<template>
  <div>
    <div class="todo-container">
      <div class="todo-card">
        <h1 class="todo-title">Subtask</h1>
        <div class="todo-input-container">
          <v-text-field
            v-model="newSubtask"
            class="todo-input"
            prepend-icon="mdi-plus"
            rounded
            label="Create a new todo..."
            type="text"
            @keydown.enter.prevent="createSubtask"
            dense
            hide-details="auto"
            dark
          />
        </div>
        <!-- main-container -->
        <div class="todo-tasks-container">
          <!-- checkbox -->
          <div v-for="task in displayedTasks" :key="task.id">
            <TaskItem
              :taskDetails="task"
              @check="updateCompletedStatus()"
              @delete="confirmDelete(task)"
            />
          </div>

          <div class="todo-filters">
            <p class="todo-incomplete-count">
              {{
                incompleteTaskCount > 1
                  ? `${incompleteTaskCount} items left`
                  : `${incompleteTaskCount} item left`
              }}
            </p>
            <div class="todo-filters-buttons">
              <div v-if="!isMobile">
                <v-btn text x-small dark color="primary">All</v-btn>
                <v-btn text x-small dark color="rgb(161, 161, 161)"
                  >Active</v-btn
                >
                <v-btn text x-small dark color="rgb(161, 161, 161)"
                  >Completed</v-btn
                >
              </div>
              <v-btn text x-small dark color="rgb(161, 161, 161)"
                >Clear Completed</v-btn
              >
            </div>
          </div>
        </div>

        <div v-if="isMobile" class="todo-filters-mobile">
          <v-btn text x-small dark color="primary">All</v-btn>
          <v-btn text x-small dark color="rgb(161, 161, 161)">Active</v-btn>
          <v-btn text x-small dark color="rgb(161, 161, 161)">Completed</v-btn>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TaskItem from "./TaskItem.vue";

export default {
  name: "Task",
  components: { TaskItem },
  data: () => ({
    currentFilter: "all",
    task: {},
    allTasks: [],
    newSubtask: "",
  }),
  methods: {
    createSubtask() {
      if (!this.newSubtask) return;

      // adds a new subtask to the current task
      this.task.subtasks.push({
        id: Math.floor(Math.random() * 10000),
        task: this.newSubtask,
        isComplete: false,
      });

      // updates the specific task inside the tasks array (array of all of the tasks)
      this.allTasks = this.allTasks.map((task) => {
        return task.id === this.task.id ? this.task : task;
      });

      this.newSubtask = "";

      // updates the todo-list in the localStorage with updated tasks and subtasks
      localStorage.setItem("todo-list", JSON.stringify(this.allTasks));
    },
    updateCompletedStatus() {
      // updates the specific task inside the tasks array (array of all of the tasks)
      this.allTasks = this.allTasks.map((task) => {
        return task.id === this.task.id ? this.task : task;
      });

      // updates the todo-list in the localStorage with updated tasks and subtasks
      localStorage.setItem("todo-list", JSON.stringify(this.allTasks));
    },
    confirmDelete(task) {
      console.log(task);
    },
  },
  computed: {
    displayedTasks() {
      switch (this.currentFilter) {
        case "active":
          return this.task.subtasks.filter((task) => !task.isComplete);
        case "completed":
          return this.task.subtasks.filter((task) => task.isComplete);
        default:
          return this.task.subtasks;
      }
    },
    incompleteTaskCount: function () {
      if (!this.task.subtasks) {
        return 0;
      }

      let count = this.task.subtasks.filter((task) => {
        return task.isComplete === false;
      });

      return count.length;
    },
    isMobile: function () {
      return this.$vuetify.breakpoint.mobile;
    },
  },
  mounted() {
    const tasks = localStorage.getItem("todo-list");
    const taskId = this.$route.params.id;

    // finds the specific task in with the same id as the route parameter
    const task = JSON.parse(tasks).find((item) => {
      return Number(item.id) === Number(taskId);
    });

    this.task = task;
    this.allTasks = JSON.parse(tasks);
  },
};
</script>

<style scoped>
.todo-container {
  background: linear-gradient(45deg, blue, blueviolet, rgb(173, 21, 173));
  position: relative;
  height: 30vh;
  color: white;
}

.todo-card {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  min-width: 300px;
  max-width: 500px;
  width: 50%;
  position: absolute;
  left: 50%;
  margin-top: 5rem;
  transform: translateX(-50%);
}

.todo-title {
  font-weight: 700;
  letter-spacing: 5px;
  /*color: white;*/
}

.todo-input-container {
  background-color: rgb(54, 54, 87);
  padding: 15px;
  border-radius: 2.5px;
}

.todo-tasks-container {
  background-color: rgb(54, 54, 87);
  border-radius: 2.5px;
}

.todo-filters {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 15px;
}

.todo-filters-mobile {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgb(54, 54, 87);
  padding: 10px;
  border-radius: 2.5px;
}

.todo-filters p {
  margin: 0;
  font-size: 0.75rem;
  color: rgb(161, 161, 161);
}

.todo-filters-buttons {
  display: flex;
  align-items: center;
  gap: 2rem;
}

/* test styles */
.hover-test {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 75px;
  background-color: rgb(44, 44, 73);
}
</style>

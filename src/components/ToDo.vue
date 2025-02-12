<template>
  <div>
    <div class="todo-container">
      <div class="todo-card">
        <h1 class="todo-title">TODO</h1>
        <div class="todo-input-container">
          <v-text-field
            v-model="newTask"
            class="todo-input"
            prepend-icon="mdi-plus"
            rounded
            label="Create a new todo..."
            type="text"
            @keydown.enter.prevent="createTask"
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
                <v-btn
                  text
                  x-small
                  dark
                  color="primary"
                  @click="showTasks('all')"
                  >All</v-btn
                >
                <v-btn
                  text
                  x-small
                  dark
                  color="rgb(161, 161, 161)"
                  @click="showTasks('active')"
                  >Active</v-btn
                >
                <v-btn
                  text
                  x-small
                  dark
                  color="rgb(161, 161, 161)"
                  @click="showTasks('completed')"
                  >Completed</v-btn
                >
              </div>
              <v-btn
                text
                x-small
                dark
                color="rgb(161, 161, 161)"
                @click="clearCompletedTasks"
                >Clear Completed</v-btn
              >
            </div>
          </div>
        </div>

        <div v-if="isMobile" class="todo-filters-mobile">
          <v-btn text x-small dark color="primary" @click="showTasks('all')"
            >All</v-btn
          >
          <v-btn
            text
            x-small
            dark
            color="rgb(161, 161, 161)"
            @click="showTasks('active')"
            >Active</v-btn
          >
          <v-btn
            text
            x-small
            dark
            color="rgb(161, 161, 161)"
            @click="showTasks('completed')"
            >Completed</v-btn
          >
        </div>
        <!-- delete task dialog -->
        <Dialog
          :open="dialog.delete"
          :task="taskToDelete.task"
          @toggle="closeDialog('delete')"
          @agree="deleteTask()"
        />
      </div>
    </div>

    <!-- delete success snackbar -->
    <Snackbar
      :open="snackbar.success.isOpen"
      :message="snackbar.success.message"
      @close="closeSnackbar('success')"
    />
  </div>
</template>

<script>
import Dialog from "./Dialog.vue";
import Snackbar from "./Snackbar.vue";
import TaskItem from "./TaskItem.vue";

export default {
  name: "ToDo",

  components: {
    Dialog,
    Snackbar,
    TaskItem,
  },
  data: () => ({
    newTask: "",
    currentFilter: "all",
    taskToDelete: {},
    tasks: [],
    dialog: {
      delete: false,
    },

    snackbar: {
      success: {
        isOpen: false,
        message: "Deleted successfully",
      },
    },
  }),
  methods: {
    createTask() {
      if (!this.newTask) return;

      this.tasks.push({
        id: Math.floor(Math.random() * 10000),
        task: this.newTask,
        isComplete: false,
        subtasks: [],
      });

      this.newTask = "";

      localStorage.setItem("todo-list", JSON.stringify(this.tasks));
    },

    updateCompletedStatus() {
      localStorage.setItem("todo-list", JSON.stringify(this.tasks));
    },

    showTasks(action) {
      this.currentFilter = action;
    },

    clearCompletedTasks() {
      let tasks = this.tasks.filter((task) => {
        return task.isComplete === false;
      });

      this.tasks = tasks;
      this.currentFilter = "all";
      localStorage.setItem("todo-list", JSON.stringify(this.tasks));
    },

    confirmDelete(task) {
      this.taskToDelete = task;
      this.openDialog("delete");
    },

    deleteTask() {
      this.tasks = this.tasks.filter((task) => {
        return task.id !== this.taskToDelete.id;
      });

      this.currentFilter = "all";
      localStorage.setItem("todo-list", JSON.stringify(this.tasks));

      this.closeDialog("delete");
      this.openSnackbar("success", "Task deleted successfully");
    },

    openDialog(dialog) {
      this.dialog[dialog] = true;
    },

    closeDialog(dialog) {
      this.dialog[dialog] = false;
    },

    openSnackbar(type, message) {
      this.snackbar[type] = {
        isOpen: true,
        message: message,
      };
    },

    closeSnackbar(type) {
      this.snackbar[type] = {
        isOpen: false,
        message: "",
      };
    },
  },
  computed: {
    displayedTasks() {
      switch (this.currentFilter) {
        case "active":
          return this.tasks.filter((task) => !task.isComplete);
        case "completed":
          return this.tasks.filter((task) => task.isComplete);
        default:
          return this.tasks;
      }
    },
    incompleteTaskCount: function () {
      let count = this.tasks.filter((task) => {
        return task.isComplete === false;
      });

      return count.length;
    },

    allTasks: function () {
      return this.tasks;
    },

    activeTasks: function () {
      let tasks = this.tasks.filter((task) => {
        return !task.isComplete;
      });

      return tasks;
    },

    completedTasks: function () {
      let tasks = this.tasks.filter((task) => {
        return task.isComplete;
      });

      return tasks;
    },
    isMobile: function () {
      return this.$vuetify.breakpoint.mobile;
    },
  },
  mounted() {
    const tasks = localStorage.getItem("todo-list");
    if (!tasks) {
      this.tasks = [];
    } else {
      this.tasks = JSON.parse(tasks);
    }
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

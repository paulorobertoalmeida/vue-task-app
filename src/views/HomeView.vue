<template>
  <div class="container">
    <Header
      title="Task Tracker"
      @toggle-task-form="toggleTaskForm"
      :showAddTask="showAddTask"
    />
    <div v-if="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks
      @delete-task="deleteTask"
      @toggle-reminder="toggleReminder"
      :tasks="tasks"
    />
  </div>
</template>

<script>
import Header from "../components/Header.vue";
import Tasks from "../components/Tasks.vue";
import AddTask from "../components/AddTask.vue";

export default {
  name: "HomeView",
  components: { Header, Tasks, AddTask },
  data() {
    return {
      tasks: [],
      showAddTask: false,
    };
  },
  methods: {
    // Delete Task Function - BASIC - NO DB CONNECTION
    // deleteTask(id) {
    //   if (confirm("Are you sure you want to delete this task?, WU TANG ")) {
    //     this.tasks = this.tasks.filter((task) => task.id !== id);
    //   }
    // },

    // Delete Task Function - BASIC - NO DB CONNECTION
    async deleteTask(id) {
      if (confirm("Are you sure you want to delete this task?, WU TANG ")) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });

        res.status === 200
          ? (this.taks = this.taks.filter((task) => task.id !== id))
          : alert(
              "Hey There has been an error deleting your Task, please try again later."
            );
      }

      if (confirm()) {
        this.tasks = this.tasks.filter((task) => task.id !== id);
      }
    },

    // Toggle Reminder Function - BASIC
    // toggleReminder(id) {
    //   this.tasks = this.tasks.map((task) =>
    //     task.id === id ? { ...task, reminder: !task.reminder } : task
    //   );
    // },
    // Toggle Reminder Function - ASYNC FUNCTION WITH DB
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updTask),
      });

      const data = await res.json();

      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },

    // Toggle Task Form to input new task
    toggleTaskForm() {
      this.showAddTask = !this.showAddTask;
    },
    // Add Task Function que recibe un customEvent de el hijo [AddTask.vue component], y debido a que no estamos trabajando con una base de datos, estaremos en esta instancia, empujando la nueva traea al array de tareas existentes!
    // addTask(id) {
    //   this.tasks = [...this.tasks, id];
    // },

    // Add Task function para empujar nuevas tareas mediante una funcion asyncornica dentro de nuestro db.json[base de datos].
    async addTask(task) {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });
    },

    // fetch ALL Tasks mediante el API
    async fetchTasks() {
      const res = await fetch("api/tasks");
      const data = await res.json();

      return data;
    },
    // fetch individual Task mediante el API
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();

      return data;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>

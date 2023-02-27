<template>
  <v-card max-width="448" class="mx-auto" color="grey-lighten-3">
    <v-layout>
      <v-app-bar color="teal-darken-4">
        <template v-slot:image>
          <v-img
            gradient="to top right, rgba(19,84,122,.8), rgba(128,208,199,.8)"
          ></v-img>
        </template>
        <v-app-bar-title>TO DO LIST - {{ name }}</v-app-bar-title>
        <v-text-field
          v-if="newFilter"
          v-model="filter"
          label="filter"
        ></v-text-field>
        <v-spacer></v-spacer>
        <v-btn icon @click="newFilter = !newFilter">
          <v-icon>mdi-magnify</v-icon>
        </v-btn>
        <v-btn icon @click="newTaskForm = !newTaskForm">
          <v-icon>mdi-plus</v-icon>
        </v-btn>
        <v-btn icon @click="logout">
          <v-icon>mdi-export</v-icon>
        </v-btn>
      </v-app-bar>
      <v-main>
        <v-container fluid>
          <v-sheet v-if="newTaskForm" class="mx-auto">
            <v-form fast-fail @submit.prevent="addNewTask">
              <v-text-field v-model="title" label="title"></v-text-field>
              <v-textarea v-model="description" label="description">
              </v-textarea>
              <div v-if="titleError">{{ titleError }}</div>
              <v-btn type="submit" block class="mt-2" :disabled="isDisabled"
                >add new task</v-btn
              >
            </v-form>
          </v-sheet>
          <v-row dense v-if="!newTaskForm && tasks && tasks.length >= 1">
            <v-col v-for="task in tasks" :key="task.id" cols="12">
              <v-card class="todo-card" elevation="2">
                <v-card-text>
                  <v-layout align-center>
                    <div xs1>
                      <v-checkbox
                        @click="checkTask(task.id)"
                        v-model="task.checked"
                      ></v-checkbox>
                    </div>
                    <div xs11>
                      <v-card-title class="title">{{
                        task.title
                      }}</v-card-title>
                      <v-card-text>{{ task.description }}</v-card-text>
                    </div>
                  </v-layout>
                </v-card-text>
                <v-card-actions>
                  <v-btn @click="deleteTask(task.id)">Excluir</v-btn>
                </v-card-actions>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
      </v-main>
    </v-layout>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      newTaskForm: false,
      title: "",
      titleError: "",
      description: "",
      tasks: [],
      filter: "",
      newFilter: false,
      isDisabled: true,
    };
  },
  name: "index",
  mounted() {
    const user = JSON.parse(localStorage.getItem("user"));
    if (!user) navigateTo("/login");
    this.name = user.name;
    if (user.tasks.length > 0) this.tasks = user.tasks;
  },
  methods: {
    logout() {
      localStorage.removeItem("user");
      navigateTo("/login");
    },
    addNewTask() {
      const user = JSON.parse(localStorage.getItem("user"));
      const users = JSON.parse(localStorage.getItem("users"));
      const newTask = {
        title: this.title,
        description: this.description,
        checked: false,
      };
      users[user.id].tasks.push({
        id: users[user.id].tasks.length > 0 ? users[user.id].tasks.length : 0,
        ...newTask,
      });
      this.tasks.push({
        id: users[user.id].tasks.length > 0 ? users[user.id].tasks.length : 0,
        ...newTask,
      });
      localStorage.setItem("users", JSON.stringify(users));
      localStorage.setItem("user", JSON.stringify(users[user.id]));
      this.newTaskForm = false;
      this.title = "";
      this.description = "";
    },
    deleteTask(id) {
      const newTasks = this.tasks.filter((task) => task.id !== id);
      this.tasks = newTasks;
      const user = JSON.parse(localStorage.getItem("user"));
      const users = JSON.parse(localStorage.getItem("users"));
      users[user.id].tasks = newTasks;
      localStorage.setItem("users", JSON.stringify(users));
      localStorage.setItem("user", JSON.stringify(users[user.id]));
    },
    checkTask(id) {
      console.log("checado pae");
    },
    filterTasks() {
      const allTasks = JSON.parse(localStorage.getItem("user")).tasks;
      const tasksFiltered = allTasks.filter(
        (task) =>
          task.title.toLowerCase().includes(this.filter.toLowerCase()) ||
          task.description.toLowerCase().includes(this.filter.toLowerCase())
      );
      this.tasks = tasksFiltered;
    },
    validateNewTask() {
      if (!this.title) {
        this.titleError = "Please fill in the title of your task";
        this.isDisabled = true;
      } else {
        this.titleError = "";
        this.isDisabled = false;
      }
    },
  },
  watch: {
    filter: ["filterTasks"],
    title: ["validateNewTask"],
    description: ["validateNewTask"],
  },
};
</script>

<style>
.todo-card {
  max-width: 400px;
  margin: 20px auto;
  padding: 20px;
}
.title {
  font-size: 24px;
  margin-bottom: 8px;
}
</style>

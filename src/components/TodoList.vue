<template>
  <div class="todo-app">
    <div class="input-container">
      <input
        v-model="newTask.name"
        @keyup.enter="addTask"
        placeholder="Ajouter une tâche..."
      />
      <input
        type="date"
        v-model="newTask.date"
        @keyup.enter="addTask"
        placeholder="Date de fin"
      />
      <button @click="addTask">Ajouter</button>
    </div>
  </div>

  <!-- Filtres des tâches -->
  <div>
    <button id="filter-all" @click="setFilter('all')">Toutes</button>
    <button id="filter-done" @click="setFilter('completed')">Complètes</button>
    <button id="filter" @click="setFilter('incomplete')">Non Complètes</button>
  </div>

  <!-- Liste des tâches avec une petite transition -->
  <transition-group name="fade" tag="ul">
    <li
      v-for="(task, index) in filteredTasks"
      :key="task.name"
      :class="{
        completed: task.statut === 'fait',
        ongoing: task.statut === 'en cours',
        notStarted: task.statut === 'non débuté',
      }"
    >
      {{ task.name }} - <strong>{{ task.statut }}</strong>
      <div>
        Date de fin :
        <input type="date" v-model="task.date" @change="saveTasks" />
      </div>
      <button @click="toggleTask(index)">Basculer le statut</button>
      <button @click="removeTask(index)">Supprimer</button>
    </li>
  </transition-group>
</template>

<script lang="ts">
import { defineComponent } from "vue";

interface Task {
  name: string;
  statut: "non débuté" | "en cours" | "fait";
  date: string; // Nouvelle propriété pour la date de fin
}

export default defineComponent({
  name: "TodoList",
  data() {
    return {
      newTask: {
        name: "",
        date: "",
      },
      tasks: [] as Task[],
      filter: "all" as string,
    };
  },
  computed: {
    filteredTasks() {
      if (this.filter === "completed") {
        return this.tasks.filter((task) => task.statut === "fait");
      } else if (this.filter === "incomplete") {
        return this.tasks.filter((task) => task.statut !== "fait");
      }
      return this.tasks;
    },
  },
  methods: {
    addTask() {
      if (this.newTask.name.trim()) {
        this.tasks.push({ ...this.newTask, statut: "non débuté" });
        this.newTask.name = "";
        this.newTask.date = "";
      }
    },
    toggleTask(index: number) {
      const task = this.tasks[index];
      if (task.statut === "non débuté") {
        task.statut = "en cours";
      } else if (task.statut === "en cours") {
        task.statut = "fait";
      } else if (task.statut === "fait") {
        task.statut = "non débuté";
      }
    },
    removeTask(index: number) {
      this.tasks.splice(index, 1);
    },
    saveTasks() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    loadTasks() {
      const tasks = localStorage.getItem("tasks");
      if (tasks) {
        this.tasks = JSON.parse(tasks);
      }
    },
    setFilter(newFilter: string) {
      this.filter = newFilter;
    },
  },
  created() {
    this.loadTasks();
  },
  watch: {
    tasks: {
      handler() {
        this.saveTasks();
      },
      deep: true,
    },
  },
});
</script>

<style scoped>
/* Animation d'entrée et de sortie pour les tâches */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

/* Style pour les tâches complètes */
.completed {
  background-color: #d3ffd3; /* Vert clair pour les tâches complètes */
  color: green;
}

.ongoing {
  background-color: #ffe0b2; /* Couleur orange clair pour les tâches en cours */
  color: #ff9800;
}

.notStarted {
  background-color: #bbdefb; /* Bleu clair pour les tâches non débutées */
  color: #2196f3;
}

/* Styles pour la disposition des tâches */
ul {
  list-style-type: none;
  padding: 0;
  margin-top: 20px;
}

li {
  padding: 10px;
  margin: 5px 0;
  border: 1px solid #ddd;
  background-color: #f9f9f9;
  display: flex;
  align-items: center; /* Alignement vertical centré */
  justify-content: space-between; /* Espacement uniforme entre les éléments */
}

li > div {
  display: flex;
  align-items: center; /* Centrer verticalement */
}

li span {
  margin-right: 15px;
}

input[type="date"] {
  margin-right: 15px;
  padding: 5px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

button {
  margin: 0 5px;
  padding: 8px 12px;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  font-size: 14px;
}

button:hover {
  opacity: 0.8;
}

button#filter-all {
  background-color: #2196f3; /* Bleu */
  color: white;
}

button#filter-done {
  background-color: #4caf50; /* Vert */
  color: white;
}

button#filter {
  background-color: #ff9800; /* Orange */
  color: white;
}
</style>

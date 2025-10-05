<template>
  <div>
    <div class="container">
      <h1>Gestor de tareas</h1>
      <!-- input con el uso de v-model para agregar tareas -->
      <input
        v-model="newTask"
        placeholder="Escribe tu tarea"
        @keyup.enter="addTask"
      />
      <button @click="addTask">agregar</button>
      <hr />
    </div>
    <!-- Panel de tareas -->
    <div v-if="tasks.length > 0" class="task-panel">
      <!-- columna to-do -->
      <div class="columna">
        <h2>To do</h2>
        <ul>
          <li
            v-for="(task, index) in tasks.filter((t) => t.status === 'to-do')"
            :key="index"
            class="task-card todo"
          >
            {{ index + 1 }}. {{ task.name }}
            <button @click="moveTask(task)" class="button-move">➝</button>
          </li>
        </ul>
      </div>
      <!-- columna doing -->
      <div class="columna">
        <h2>Doing</h2>
        <ul>
          <li
            v-for="(task, index) in tasks.filter((t) => t.status === 'doing')"
            :key="index"
            class="task-card doing"
          >
            {{ index + 1 }}. {{ task.name }}
            <button @click="moveTask(task)" class="button-move">➝</button>
          </li>
        </ul>
      </div>
      <!-- columna done -->
      <div class="columna">
        <h2>Done</h2>
        <ul>
          <li
            v-for="(task, index) in tasks.filter((t) => t.status === 'done')"
            :key="index"
            class="task-card done"
          >
            {{ index + 1 }}. {{ task.name }}
          </li>
        </ul>
      </div>
    </div>
    <!-- Si no hay tareas -->
    <div v-else>
      <p>No hay tareas</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

//Modelo Variables reactivas
const newTask = ref("");
const tasks = ref([]);

const addTask = () => {
  const name = newTask.value.trim();
  if (name !== "") {
    tasks.value.push({ name, status: "to-do" });
    //limpiar campo
    newTask.value = "";
  }
};

const moveTask = (task) => {
  if (task.status === "to-do") {
    task.status = "doing";
  } else if (task.status === "doing") {
    task.status = "done";
  }
};
</script>

<style>
.container {
  max-width: 500px;
  margin: 40px auto;
  font-family: sans-serif;
}

input {
  padding: 8px;
  width: 70%;
}

button {
  margin-left: 10px;
  padding: 8px 12px;
}

.task-panel {
  display: flex;
  justify-content: space-between;
  gap: 20px;
}

ul {
  list-style: none;
  padding: 0;
}

.task-card {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-radius: 8px;
  color: white;
  padding: 10px;
  margin-bottom: 10px;
  font-weight: 500;
}

.todo {
  background-color: #3498db;
}

.doing {
  background-color: #2ecc71;
}

.done {
  background-color: #e74c3c;
}

.button-move {
  background: none;
  border: none;
  color: white;
  font-size: 20px;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.button-move:hover {
  transform: scale(1.2);
}
</style>

<script setup>
import { ref, computed, onMounted } from 'vue';


const tasks = ref([
  { name: "Bench Press", done: false },
  { name: "Bicep Curl", done: true },
  { name: "Tricep Pushdown", done: false },
  { name: "Incline Bench Press", done: true }
]);

const newTask = ref("");
const inputField = ref(null);

const isEditing = ref(false);
const editIndex = ref(null);
const showCompleted = ref(true);

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push({ name: newTask.value.trim(), done: false });
    newTask.value = "";
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
  cancelEdit();
};

const editTask = (index) => {
  newTask.value = tasks.value[index].name;
  isEditing.value = true;
  editIndex.value = index;
  inputField.value.focus();
};

const updateTask = () => {
  if (editIndex.value !== null && newTask.value.trim() !== "") {
    tasks.value[editIndex.value].name = newTask.value.trim();
    cancelEdit();
  }
};

const cancelEdit = () => {
  newTask.value = "";
  isEditing.value = false;
  editIndex.value = null;
};

const filteredTasks = computed(() => {
  return showCompleted.value
    ? tasks.value
    : tasks.value.filter(task => !task.done);
});

onMounted(() => {
  inputField.value?.focus();
});

</script>

<template>
  
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
    <a href="#" target="_blank">
      <img src="./assets/1.jpg" class="" alt="" />
    </a> 

  <div id="app">
    <!-- Navbar -->
    <nav class="navbar">
      <div class="navbar-content">
        <h1>Daftar Gerakan GYM</h1>
      </div>
    </nav> 

    <!-- Container -->
    <div class="container">
      <div class="card">
        <h2>📋 DAFTAR GERAKAN </h2>

        <label class="toggle-show">
          <input type="checkbox" v-model="showCompleted" />
          Tampilkan gerakan yang sudah selesai
        </label>

        <ul class="task-list">
          <li v-for="(task, index) in filteredTasks" :key="index">
            <div class="task-content">
              <input type="checkbox" v-model="task.done" />
              <span :class="{ done: task.done }">{{ index + 1 }}. {{ task.name }}</span>
            </div>
            <div class="action-buttons">
              <button class="edit-btn" @click="editTask(index)">✏</button>
              <button class="delete-btn" @click="deleteTask(index)">🗑</button>
            </div>
          </li>
        </ul>

        

        <div class="input-group">
          <input ref="inputField" v-model="newTask" type="text" placeholder="Masukkan kegiatan baru" />
          <button v-if="isEditing" @click="updateTask">Update</button>
          <button v-else @click="addTask">Tambah</button>
          <button v-if="isEditing" class="cancel-btn" @click="cancelEdit">Batal</button>
        </div>
      </div>
    </div>

    <!-- Footer -->
    <footer class="footer">
      <p>© 2025 Shandi Reskiawan</p>
    </footer>
  </div>
</template>


<style>
html, body, #app {
  /* margin: 0; */
  padding: 0;
  width: 100%;
  min-height: 100vh;
  font-family: 'Poppins', sans-serif;
  background: url('@/assets/22.jpg') no-repeat center center fixed;
  background-size: cover;
  color: #fff;
}



#app::before {
  content: "";
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0, 0, 0, 0.6);
  z-index: -1;
}

/* Navbar */
.navbar {
  width: 100%;
  background: rgba(0, 0, 0, 0.7);
  padding: 20px 0;
  text-align: center;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
  position: sticky;
  top: 0;
  z-index: 10;
}

.navbar-content h1 {
  margin: 0;
  font-size: 28px;
  color: #fff;
  font-weight: bold;
}

/* Container */
.container {
  display: flex;
  justify-content: center;   /* Horizontal center */
  align-items: center;       /* Vertical center */
  padding: 20px;
  min-height: calc(100vh - 140px); /* Full height minus navbar + footer */
  box-sizing: border-box;
}

.card {
  background: rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(6px);
  border-radius: 20px;
  padding: 30px 20px;
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.2);
  width: 100%;
  max-width: 600px;          /* Keep it centered and neat */
}

h2 {
  color: #fff;
  text-align: center;
  margin-bottom: 20px;
}

.toggle-show {
  display: flex;
  align-items: center;
  margin-bottom: 15px;
  font-size: 15px;
  color: #fff;
  gap: 8px;
}

/* List */
.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
  overflow-y: auto;
  flex-grow: 1;
  max-height: 300px;
}

.task-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
}

.task-content {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
}

.done {
  text-decoration: line-through;
  color: #ccc;
}

.action-buttons {
  display: flex;
  gap: 8px;
}

.edit-btn, .delete-btn {
  background: none;
  border: none;
  font-size: 18px;
  color: #fff;
  cursor: pointer;
  transition: color 0.2s;
}

.edit-btn:hover {
  color: #74c0fc;
}

.delete-btn {
  color: #ff8787;
}

.delete-btn:hover {
  color: #ff6b6b;
}

.input-group {
  display: flex;
  gap: 10px;
  margin-top: 20px;
  flex-wrap: wrap;
}

input[type="text"] {
  flex: 1;
  padding: 10px;
  border: 1px solid #dee2e6;
  border-radius: 6px;
  font-size: 15px;
  background-color: rgba(255, 255, 255, 0.9);
  color: #212529;
}

input[type="text"]:focus {
  border-color: #228be6;
  outline: none;
}

button {
  padding: 10px 12px;
  background-color: #228be6;
  color: white;
  border: none;
  border-radius: 6px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #1c7ed6;
}

.cancel-btn {
  background-color: #ff6b6b;
}

.cancel-btn:hover {
  background-color: #fa5252;
}

/* Footer */
.footer {
  width: 100%;
  background: rgba(0, 0, 0, 0.7);
  padding: 12px 20px;
  text-align: center;
  color: white;
  font-size: 14px;
  box-shadow: 0 -2px 8px rgba(0, 0, 0, 0.1);
}

/* Responsive */
@media (max-width: 600px) {
  .card {
    padding: 20px 15px;
    border-radius: 15px;
  }

  .navbar-content h1 {
    font-size: 22px;
  }

  .footer {
    font-size: 12px;
  }
}

</style>
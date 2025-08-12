<script lang="ts" setup>
import { ref } from 'vue';

const message = ref('Todo List');
const newTask = ref('');
const tasks = ref<string[]>([]);

const editingIndex = ref<number | null>(null);
const editingValue = ref('');

function formSubmitted() {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value.trim());
    newTask.value = '';
  }
}

function startEdit(index: number) {
  editingIndex.value = index;
  editingValue.value = tasks.value[index];
}

function saveEdit(index: number) {
  if (editingValue.value.trim() !== '') {
    tasks.value[index] = editingValue.value.trim();
    editingIndex.value = null;
    editingValue.value = '';
  }
}

function cancelEdit() {
  editingIndex.value = null;
  editingValue.value = '';
}

function deleteTask(index: number) {
  tasks.value.splice(index, 1);
  if (editingIndex.value === index) {
    cancelEdit();
  }
}
</script>
<template>
  <nav class="navbar">
    <div class="navbar-title">Task Manager</div>
    <ul class="navbar-links">
      <li><a href="#">Home</a></li>
      <li><a href="#">Tasks</a></li>
      <li><a href="#">Profile</a></li>
    </ul>
  </nav>
  <main>
    <div class="greeting">Hi, Abis pogi!</div>
    <h1>{{ message }}</h1>
    <form @submit.prevent="formSubmitted">
      <label>
        <span class="label-text">New Task</span>
        <input v-model="newTask" name="newTask" placeholder="Enter task" />
      </label>
      <div class="button-container">
        <button type="submit">Add</button>
      </div>
    </form>
    <ul class="task-list">
      <li v-for="(task, index) in tasks" :key="index" class="task-card">
        <template v-if="editingIndex === index">
          <input v-model="editingValue" class="edit-input" />
          <button class="save-btn" @click="saveEdit(index)">Save</button>
          <button class="cancel-btn" @click="cancelEdit">Cancel</button>
        </template>
        <template v-else>
          <span>{{ task }}</span>
          <button class="edit-btn" @click="startEdit(index)">Edit</button>
          <button class="delete-btn" @click="deleteTask(index)">Delete</button>
        </template>
      </li>
    </ul>
  </main>
</template>

<style scoped>
.navbar {
  width: 100%;
  background: linear-gradient(90deg, #2563eb 60%, #3b82f6 100%);
  color: #fff;
  padding: 0.8rem 2rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 2px 8px rgba(59, 130, 246, 0.10);
  margin-bottom: 1rem;
  border-radius: 0 0 16px 16px;
}

.navbar-title {
  font-size: 1.3rem;
  font-weight: 700;
  letter-spacing: 1px;
}

.navbar-links {
  list-style: none;
  display: flex;
  gap: 1.5rem;
  margin: 0;
  padding: 0;
}

.navbar-links a {
  color: #fff;
  text-decoration: none;
  font-weight: 500;
  transition: text-decoration 0.2s;
}

.navbar-links a:hover {
  text-decoration: underline;
}

.greeting {
  text-align: center;
  font-size: 1.3rem;
  font-weight: 600;
  color: #2563eb;
  margin-bottom: 0.5rem;
  letter-spacing: 1px;
}

/* ...existing styles below... */
body, html, #app {
  min-height: 100vh;
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', Arial, sans-serif;
  background: linear-gradient(135deg, #e0e7ff 0%, #f0fdfa 100%);
}

main {
  max-width: 420px;
  margin: 2.5rem auto;
  padding: 2rem 1.5rem;
  background: rgba(255,255,255,0.95);
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(60, 130, 246, 0.15);
  color: #222;
  position: relative;
  z-index: 1;
}

h1 {
  font-size: 2.2rem;
  margin-bottom: 1.5rem;
  color: #2563eb;
  text-align: center;
  letter-spacing: 1px;
  font-weight: 700;
}

.label-text {
  font-weight: 500;
  color: #3b82f6;
  margin-bottom: 0.25rem;
  display: block;
}

label {
  display: block;
  margin-bottom: 0.75rem;
}

input,
.edit-input {
  padding: 0.6rem 1rem;
  width: 100%;
  margin-top: 0.25rem;
  border: 1.5px solid #3b82f6;
  border-radius: 8px;
  background-color: #f5faff;
  color: #222;
  font-size: 1rem;
  transition: border-color 0.2s;
  box-shadow: 0 2px 8px rgba(59, 130, 246, 0.05);
}

input:focus,
.edit-input:focus {
  border-color: #2563eb;
  outline: none;
}

.button-container {
  display: flex;
  justify-content: flex-end;
  margin-top: 0.5rem;
}

button {
  padding: 0.6rem 2rem;
  background: linear-gradient(90deg, #3b82f6 60%, #2563eb 100%);
  color: white;
  font-weight: bold;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1rem;
  box-shadow: 0 2px 8px rgba(59, 130, 246, 0.12);
  transition: background 0.2s, transform 0.2s;
}

button:hover {
  background: linear-gradient(90deg, #2563eb 60%, #3b82f6 100%);
  transform: translateY(-2px) scale(1.04);
}

.task-list {
  margin-top: 2rem;
  padding-left: 0;
  list-style: none;
}

.task-card {
  background: linear-gradient(90deg, #f0fdfa 0%, #e0e7ff 100%);
  border-radius: 10px;
  box-shadow: 0 2px 12px rgba(59, 130, 246, 0.08);
  margin-bottom: 1rem;
  padding: 0.75rem 1rem;
  color: #222;
  font-size: 1.08rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  animation: fadeIn 0.4s;
  transition: box-shadow 0.2s;
  
}

.task-card:hover {
  box-shadow: 0 4px 16px rgba(59, 130, 246, 0.18);
  background: linear-gradient(90deg, #e0e7ff 0%, #f0fdfa 100%);
}

.edit-btn,
.save-btn,
.cancel-btn,
.delete-btn {
  padding: 0.4rem 1rem;
  margin-left: 0.5rem;
  font-size: 0.95rem;
  border-radius: 6px;
  border: none;
  cursor: pointer;
  font-weight: 500;
  transition: background 0.2s;
}

.edit-btn {
  background: #161616;
  color: #e9e0e0;
}
.edit-btn:hover {
  background: #1a1919;
}

.save-btn {
  background: #22c55e;
  color: #fff;
}
.save-btn:hover {
  background: #16a34a;
}

.cancel-btn {
  background: #0f0f0f;
  color: #fff;
}
.cancel-btn:hover {
  background: #111111;
}

.delete-btn {
  background: #181616;
  color: #f3eeee;
}
.delete-btn:hover {
  background: #181717;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(12px);}
  to { opacity: 1; transform: translateY(0);}
}
</style>
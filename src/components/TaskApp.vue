<script setup>
import { ref, computed } from 'vue'

const newTitle = ref('')
const showOnlyPending = ref(false)
const tasks = ref([
  { id: 1, title: 'Passejar al gos', done: false, createdAt: new Date() },
  { id: 2, title: "Estudiar per l'examen", done: false, createdAt: new Date() },
  { id: 3, title: 'Anar al gimnas', done: true, createdAt: new Date() }
])

const visibleTasks = computed(() => {
  return showOnlyPending.value ? tasks.value.filter((t) => !t.done) : tasks.value
})
const totalCount = computed(() => tasks.value.length)
const pendingCount = computed(() => tasks.value.filter((t) => !t.done).length)

function addTask() {
  const title = newTitle.value.trim()
  if (!title) return
  const nextId = tasks.value.length ? Math.max(...tasks.value.map((t) => t.id)) + 1 : 1
  tasks.value.push({
    id: nextId,
    title,
    done: false,
    createdAt: new Date(),
  })
  newTitle.value = ''
}

function toggleDone(task) {
  task.done = !task.done
}

function removeTask(id) {
  tasks.value = tasks.value.filter((t) => t.id !== id)
}

function formatDate(d) {
  const date = d instanceof Date ? d : new Date(d)
  return date.toLocaleString()
}
</script>

<template>
  <section class="task-app">
    <h1>Gestor de Tasques</h1>

    <div class="add-row">
      <input
        v-model="newTitle"
        type="text"
        placeholder="Títol de la tasca"
        @keyup.enter="addTask"
        aria-label="Nou títol"
      />
      <button class="primary" @click="addTask">Afegir</button>
    </div>

    <div class="controls">
      <label class="toggle">
        <input type="checkbox" v-model="showOnlyPending" />
        <span>Mostrar només pendents</span>
      </label>
      <div class="summary">
        <strong>Total:</strong> {{ totalCount }} · <strong>Pendents:</strong> {{ pendingCount }}
      </div>
    </div>

    <div v-if="visibleTasks.length" class="list">
      <div
        v-for="task in visibleTasks"
        :key="task.id"
        class="item"
        :class="{ done: task.done }"
      >
        <label class="left">
          <input type="checkbox" :checked="task.done" @change="toggleDone(task)" />
          <span class="title">{{ task.title }}</span>
        </label>

        <div class="right">
          <time class="date" :title="formatDate(task.createdAt)">
            {{ formatDate(task.createdAt) }}
          </time>
          <button class="danger" @click="removeTask(task.id)" aria-label="Eliminar">Eliminar</button>
        </div>
      </div>
    </div>

    <p v-else class="empty">No hi ha tasques a mostrar.</p>
  </section>
</template>

<style scoped>
.task-app {
  max-width: 700px;
  margin: 2rem auto;
  padding: 2rem;
  border-radius: 20px;
  background: var(--color-background-soft);
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.08);
  transition: background 0.3s, color 0.3s;
}

h1 {
  margin-bottom: 1.5rem;
  text-align: center;
  font-size: 1.8rem;
  font-weight: 600;
  color: var(--color-heading);
}

.add-row {
  display: flex;
  gap: 0.5rem;
}

.add-row input {
  flex: 1;
  padding: 0.7rem 1rem;
  border-radius: 10px;
  border: 1px solid var(--color-border);
  font-size: 1rem;
  outline: none;
  transition: border 0.2s, box-shadow 0.2s;
}
.add-row input:focus {
  border-color: #3b82f6;
  box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.2);
}

button {
  border: none;
  padding: 0.6rem 1rem;
  border-radius: 10px;
  cursor: pointer;
  font-weight: 500;
  transition: background 0.25s, transform 0.15s;
}
button:hover {
  transform: translateY(-1px);
}

button.primary {
  background: #3b82f6;
  color: white;
}
button.primary:hover {
  background: #2563eb;
}

button.danger {
  background: #ef4444;
  color: white;
  padding: 0.4rem 0.8rem;
}
button.danger:hover {
  background: #dc2626;
}

.controls {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 1rem;
  font-size: 0.95rem;
  flex-wrap: wrap;
  gap: 0.75rem;
}

.toggle {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  cursor: pointer;
}

.summary {
  opacity: 0.85;
}

.list {
  margin-top: 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.9rem 1rem;
  border: 1px solid var(--color-border);
  border-radius: 12px;
  background: var(--color-background);
  transition: background 0.2s, transform 0.1s;
}
.item:hover {
  background: var(--color-background-mute);
  transform: scale(1.01);
}

.item.done .title {
  text-decoration: line-through;
  opacity: 0.6;
}

.item .left {
  display: flex;
  align-items: center;
  gap: 0.6rem;
}

.item .right {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  font-size: 0.9rem;
}

.date {
  opacity: 0.8;
  font-size: 0.85rem;
}

.empty {
  margin-top: 1.5rem;
  text-align: center;
  opacity: 0.7;
  font-style: italic;
}
</style>

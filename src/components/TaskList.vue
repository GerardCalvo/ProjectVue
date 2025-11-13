<script setup>
import TaskItem from './TaskItem.vue'

const props = defineProps({
  tasks: {
    type: Array,
    required: true,
  },
  formatDate: {
    type: Function,
    required: true,
  },
})

const emit = defineEmits(['toggle-task', 'remove-task'])
</script>

<template>
  <div v-if="tasks.length" class="list">
    <TaskItem
      v-for="task in tasks"
      :key="task.id"
      :task="task"
      :format-date="formatDate"
      @toggle-task="emit('toggle-task', $event)"
      @remove-task="emit('remove-task', $event)"
    />
  </div>

  <p v-else class="empty">No hi ha tasques a mostrar.</p>
</template>

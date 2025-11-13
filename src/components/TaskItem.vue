<script setup>
const props = defineProps({
  task: {
    type: Object,
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
  <div
    class="item"
    :class="{ done: task.done }"
  >
    <label class="left">
      <input
        type="checkbox"
        :checked="task.done"
        @change="emit('toggle-task', task)"
      />
      <span class="title">{{ task.title }}</span>
    </label>

    <div class="right">
      <time class="date" :title="formatDate(task.createdAt)">
        {{ formatDate(task.createdAt) }}
      </time>
      <button
        class="danger"
        @click="emit('remove-task', task.id)"
        aria-label="Eliminar"
      >
        Eliminar
      </button>
    </div>
  </div>
</template>

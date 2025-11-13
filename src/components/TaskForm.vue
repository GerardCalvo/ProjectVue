<script setup>
import { ref } from 'vue'

const emit = defineEmits(['add-task'])
const newTitle = ref('')

function submit() {
  const title = newTitle.value.trim()
  if (!title) return
  emit('add-task', title)
  newTitle.value = ''
}
</script>

<template>
  <div class="add-row">
    <input
      v-model="newTitle"
      type="text"
      placeholder="Títol de la tasca"
      @keyup.enter="submit"
      aria-label="Nou títol"
    />
    <button class="primary" @click="submit">Afegir</button>
  </div>
</template>

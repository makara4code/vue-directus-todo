<script setup lang="ts">
import { onMounted, ref } from 'vue'
import type { Todo } from '@/types/todo'
const todos = ref<Todo[]>([])

const fetchTodos = async () => {
  const res = await fetch('/api/items/todos')
  const jsonRes = await res.json()

  todos.value = jsonRes?.data ?? []
}

onMounted(() => {
  fetchTodos()
})
</script>

<template>
  <div>
    <div v-for="{ title, description, id } in todos" :key="id">
      <h2>{{ title }}</h2>
      <p>{{ description }}</p>
    </div>
  </div>
</template>

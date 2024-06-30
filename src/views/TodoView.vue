<script setup lang="ts">
import { onMounted, ref } from 'vue'
import type { Todo } from '@/types/todo'
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card"
import { cn } from '@/lib/utils';
import axios from "axios";

const todos = ref<Todo[]>([])


const fetchTodos = async () => {
  const res = await fetch('/api/items/todos')
  const jsonRes = await res.json()

  todos.value = jsonRes?.data ?? []
}

const handleOnClickTodo = async (todo: Todo) => {
  const res = await axios.patch(`/api/items/todos/${todo.id}`, {
    ...todo,
    isDone: !todo.isDone
  })

  if (res.status === 200) {
    fetchTodos()
  }
}

onMounted(() => {
  fetchTodos()
})
</script>

<template>
  <div class="flex flex-col gap-4">
    <Card :class="cn(
      'p-2 cursor-pointer',
      todo.isDone ? 'bg-green-300' : ''
    )" v-for="todo in todos" :key="todo.id" @click="() => handleOnClickTodo(todo)">
      <CardHeader>
        <CardTitle>{{ todo.title }}</CardTitle>
      </CardHeader>
      <CardContent>
        <CardDescription>
          {{ todo.description }}
        </CardDescription>
      </CardContent>
    </Card>
  </div>
</template>

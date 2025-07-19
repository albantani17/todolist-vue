<script setup lang="ts">
import { ref, watchEffect } from 'vue'
import InputTask from './components/InputTask.vue'
import type { ITask } from './types/Task'
import CardTask from './components/CardTask.vue'

const tasks = ref<ITask[]>([])

watchEffect(() => {
  const tasksFromStorage = localStorage.getItem('task')
  console.log(tasksFromStorage)
  tasks.value = tasksFromStorage ? JSON.parse(tasksFromStorage) : []
})

const handleUpdate = (idTask: number) => {
  tasks.value = tasks.value.map((task) => {
    if (task.id === idTask) {
      task.isDone = !task.isDone
    }
    return task
  })
  console.log(tasks.value)
  localStorage.setItem('task', JSON.stringify(tasks.value))
}
</script>

<template>
  <div class="w-full h-[30%] absolute z-0 lg:rounded-b-[20%] bg-orange-500" />
  <div class="w-full h-[40%] absolute z-1 lg:rounded-b-[20%] bg-orange-500 opacity-25" />
  <div class="w-full h-screen z-50 relative lg:px-50 px-4 py-20">
    <div class="flex flex-col items-center mb-10 text-white">
      <h1 class="text-4xl font-poppins font-bold">TO DO LIST</h1>
      <h2 class="text-2xl font-poppins font-semibold">By Alban</h2>
    </div>
    <InputTask />
    <div class="flex flex-col gap-4 my-10">
      <CardTask
        v-for="task in tasks"
        :key="task.id"
        :task="task"
        :onUpdate="() => handleUpdate(task.id)"
      />
    </div>
  </div>
</template>

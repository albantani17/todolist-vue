<script setup lang="ts">
import type { ITask } from '@/types/Task'
import { computed, type Prop } from 'vue'

interface Props {
  task: ITask
  onUpdate: () => void
}

const { task, ...props } = defineProps<Props>()

const formattedDate = computed(() => {
  const date = new Date(task.createdAt)
  return date.toLocaleString('id-ID', {
    day: '2-digit',
    month: '2-digit',
    year: 'numeric',
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit',
    hour12: false, // ganti ke true kalau mau format 12‑jam
    timeZone: 'Asia/Jakarta', // pastikan pakai zona waktu lokal
  })
})
</script>

<template>
  <div class="flex w-full gap-2 p-4 justify-between rounded-lg border-[#e5e7eb] border-2 shadow-md">
    <div>
      <h1 class="font-poppins font-semibold" :class="{ 'line-through': task.isDone }">
        {{ task.title }}
      </h1>
      <p class="text-gray-400 text-sm">{{ formattedDate }}</p>
    </div>
    <div>
      <button
        class="bg-orange-500 p-4 rounded-lg text-white cursor-pointer hover:bg-blue-600 transition duration-300 disabled:bg-orange-200"
        type="button"
        @click="() => props.onUpdate()"
        :disabled="task.isDone"
      >
        {{ task.isDone ? 'Sudah Selesai' : 'Selesaikan' }}
      </button>
    </div>
  </div>
</template>

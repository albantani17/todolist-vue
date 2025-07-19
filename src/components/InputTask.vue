<script setup lang="ts">
import type { ITask } from '@/types/Task';
import { ref, watchEffect } from 'vue';

const task = ref('');
const errorMessage = ref('');

const emit = defineEmits<{ (e: 'add', task: ITask): void }>();

const localStorage = window.localStorage;

const addTask = () => {
  if (!task.value.trim()) {
    errorMessage.value = 'Task tidak boleh kosong';
    return;
  }
  const tasksFromStorage = localStorage.getItem('task');
  const existingTasks = tasksFromStorage ? JSON.parse(tasksFromStorage) : [];

  // 2. Buat objek task baru
  const newTask = {
    id: existingTasks.length > 0 ? Math.max(...existingTasks.map((t: any) => t.id)) + 1 : 1,
    createdAt: Date.now(),
    title: task.value,
    isDone: false,
  };
  task.value = '';
  errorMessage.value = '';
  emit('add', newTask);
};
</script>

<template>
  <div class="w-full bg-white rounded-lg shadow-xl flex flex-col gap-2 p-4">
    <div class="flex w-full gap-2">
      <input
        type="text"
        v-model="task"
        placeholder="Tambahkan Task"
        class="w-full rounded-md p-2 bg-white border-2 border-[#e5e7eb] focus:outline-none"
      />
      <button
        class="bg-orange-500 p-4 rounded-lg text-white cursor-pointer hover:bg-orange-600 transition duration-300"
        type="button"
        @click="addTask"
      >
        Tambah
      </button>
    </div>
    <p class="text-red-500" v-if="errorMessage">{{ errorMessage }}</p>
  </div>
</template>

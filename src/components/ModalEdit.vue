<script setup lang="ts">
import type { ITask } from '@/types/Task';
import { ref } from 'vue';

interface Props {
  isOpen: boolean;
  onClose: () => void;
  task: ITask;
}

// 1. Define the emit event
const emit = defineEmits<{ (e: 'update'): void }>();

const props = defineProps<Props>();
const task = ref(props.task.title);

const local = window.localStorage;

const handleUpdate = () => {
  const tasksFromStorage = local.getItem('task');
  const existingTasks = tasksFromStorage ? JSON.parse(tasksFromStorage) : [];

  // Find and update the specific task
  const taskToUpdate = existingTasks.find((t: any) => t.id === props.task.id);
  if (taskToUpdate) {
    taskToUpdate.title = task.value;
  }

  local.setItem('task', JSON.stringify(existingTasks));

  // 2. Emit the 'update' event to the parent (CardTask)
  emit('update');
  props.onClose(); // Close the modal after emitting
};
</script>

<template>
  <template v-if="props.isOpen">
    <div class="absolute top-0 left-0 w-full h-full bg-black opacity-50"></div>
    <div
      class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-1/2 bg-white p-4 rounded-xl"
    >
      <div class="flex justify-between mb-4">
        <h1 class="text-2xl font-poppins font-semibold">Edit Task</h1>
        <button class="cursor-pointer text-2xl" type="button" @click="props.onClose">
          &times;
        </button>
      </div>
      <div>
        <label for="task" class="block mb-2 text-xl text-black">Judul Task</label>
        <input
          id="task"
          type="text"
          v-model="task"
          placeholder="Tambahkan Task"
          class="w-full rounded-md p-2 bg-white border-2 border-[#e5e7eb] focus:outline-none"
        />
      </div>
      <div class="flex justify-end mt-4 gap-2">
        <button
          class="bg-orange-500 p-4 rounded-lg text-white cursor-pointer hover:bg-orange-600 transition duration-300"
          type="button"
          @click="props.onClose"
        >
          Batal
        </button>
        <button
          class="bg-orange-500 p-4 rounded-lg text-white cursor-pointer hover:bg-orange-600 transition duration-300"
          type="button"
          @click="handleUpdate"
        >
          Simpan
        </button>
      </div>
    </div>
  </template>
</template>

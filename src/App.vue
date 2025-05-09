<template>
  <div class="todo-container">
  <h1>📋 Daftar Kegiatan</h1>
  <form @submit.prevent="addTask">
    <input v-model="newTask" type="text" placeholder="Tambahkan kegiatan..." />
    <button type="submit">Tambah</button>
  </form>

  <div class="filter">
    <span>Total: {{ tasks.length }}</span>
    <label><input type="checkbox" v-model="showOnlyPending" /> Hanya yang belum selesai</label>
  </div>

  <ul>
    <li v-for="(task, index) in filteredTasks" :key="index">
      <div class="task-title">
        <input type="checkbox" v-model="task.done" />
        <span :class="{ done: task.done }">{{ task.title }}</span>
      </div>
      <button @click="removeTask(index)">Hapus</button>
    </li>
  </ul>
</div>

</template>

<script setup>
import { ref, computed } from 'vue'

const newTask = ref('')
const tasks = ref([])
const showOnlyPending = ref(false)

const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push({ title: newTask.value, done: false })
    newTask.value = ''
  }
}

const removeTask = (index) => {
  tasks.value.splice(index, 1)
}

const filteredTasks = computed(() =>
  showOnlyPending.value ? tasks.value.filter(t => !t.done) : tasks.value
)
</script>

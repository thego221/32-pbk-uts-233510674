<template>
  <div class="todo-container">
    <h1>📋 Daftar Kegiatan</h1>

    <form @submit.prevent="addTask" class="form">
      <input v-model="newTask" type="text" placeholder="Tambahkan kegiatan..." />
      <button type="submit">Tambah</button>
    </form>

    <div class="filter-dropdown" v-click-outside="closeDropdown">
      <button @click="dropdownOpen = !dropdownOpen" class="filter-toggle">
        Filter: {{ showOnlyPending ? 'Belum Selesai' : 'Semua' }}
      </button>
      <div v-if="dropdownOpen" class="dropdown-menu">
        <ul>
          <li @click="setFilter(false)">Tampilkan Semua</li>
          <li @click="setFilter(true)">Tampilkan Belum Selesai</li>
        </ul>
      </div>
    </div>

    <ul>
      <li v-for="(task, index) in filteredTasks" :key="index" class="task-item">
        <div class="task-title">
          <input type="checkbox" v-model="task.done" />
          <span :class="{ done: task.done }">{{ task.title }}</span>
        </div>
        <button @click="removeTask(index)" class="delete-button">Hapus</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const tasks = ref([
  { title: 'Belajar Vue.js', done: false },
  { title: 'Mengerjakan UTS', done: false },
  { title: 'Membaca Buku', done: true } 
])
const newTask = ref('')
const showOnlyPending = ref(false)
const dropdownOpen = ref(false)

const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push({ title: newTask.value, done: false })
    newTask.value = ''
  }
}

const removeTask = (index) => {
  tasks.value.splice(index, 1)
}

const setFilter = (pendingOnly) => {
  showOnlyPending.value = pendingOnly
  dropdownOpen.value = false
}

const closeDropdown = () => {
  dropdownOpen.value = false
}

const filteredTasks = computed(() => {
  return showOnlyPending.value
    ? tasks.value.filter(task => !task.done)
    : tasks.value
})
</script>

<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'

const tasks = ref([])
const loading = ref(true)
const error = ref(false)

async function loadTasks() {
  loading.value = true
  error.value = false

  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/todos')
    tasks.value = response.data
  } catch {
    error.value = true
  } finally {
    loading.value = false
  }
}

onMounted(loadTasks)
</script>

<template>
  <main class="container py-5">
    <h1 class="mb-4">Opgaveliste</h1>

    <p v-if="loading" role="status">Henter opgaver...</p>
    <div v-else-if="error" class="alert alert-danger" role="alert">
      Opgaverne kunne ikke hentes.
      <button type="button" class="btn btn-outline-danger btn-sm ms-2" @click="loadTasks">
        Prøv igen
      </button>
    </div>
    <div v-else class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-4">
      <div v-for="task in tasks" :key="task.id" class="col">
        <div class="card h-100">
          <div class="card-body">
            <h2 class="card-title h5">{{ task.title }}</h2>
            <span :class="['badge', task.completed ? 'text-bg-success' : 'text-bg-secondary']">
              {{ task.completed ? 'Færdig' : 'Ikke færdig' }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

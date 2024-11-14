<script setup>
import { ref, onMounted, computed } from 'vue'
import axios from 'axios'

defineProps({
  msg: String,
})

const projects = ref([])
const newProject = ref({
  title: '',
  description: '',
  image: '',
  technologies: '',
  category_id: ''
})
const showForm = ref(false)
const categories = ref([])
const loading = ref(false)
const error = ref(null)
const selectedCategory = ref('all')

const createProject = async () => {
  loading.value = true
  error.value = null

  try {
    const techArray = newProject.value.technologies.split(',').map(tech => tech.trim())

    const response = await axios.post('http://127.0.0.1:8000/api/projects', {
      ...newProject.value,
      technologies: techArray,
      category_id: parseInt(newProject.value.category_id)
    })

    projects.value.unshift(response.data)

    newProject.value = {
      title: '',
      description: '',
      image: '',
      technologies: '',
      category_id: ''
    }
    showForm.value = false
  } catch (error) {
    error.value = 'Si è verificato un errore durante la creazione del progetto'
    console.error('Error creating project:', error)
  } finally {
    loading.value = false
  }
}

const filteredProjects = computed(() => {
  if (selectedCategory.value === 'all') return projects.value
  return projects.value.filter(project => project.category_id === parseInt(selectedCategory.value))
})

onMounted(async () => {
  loading.value = true
  try {
    const [projectsResponse, categoriesResponse] = await Promise.all([
      axios.get('http://127.0.0.1:8000/api/projects'),
      axios.get('http://127.0.0.1:8000/api/categories')
    ])
    projects.value = projectsResponse.data
    categories.value = categoriesResponse.data
  } catch (error) {
    error.value = 'Si è verificato un errore nel caricamento dei dati'
    console.error('Error fetching data:', error)
  } finally {
    loading.value = false
  }
})
</script>

<template>
  <div class="text-center container">
    <div class="hero-section mb-5 p-5 rounded-4 bg-gradient text-white">
      <h1 class="display-3 mb-4 fw-bold">{{ msg }}</h1>
      <p class="lead mb-0">Esplora i progetti e crea nuove idee innovative</p>
    </div>

    <div v-if="error" class="alert alert-danger mb-4 shadow-sm" role="alert">
      {{ error }}
    </div>

    <div class="d-flex justify-content-between align-items-center mb-5">
      <button class="btn btn-primary px-5 py-3 rounded-pill shadow-sm hover-lift" @click="showForm = !showForm"
        :disabled="loading">
        <span v-if="loading" class="spinner-border spinner-border-sm me-2" role="status"></span>
        {{ showForm ? 'Chiudi Form' : 'Nuovo Progetto' }}
      </button>

      <div class="category-filter">
        <select class="form-select rounded-pill" v-model="selectedCategory">
          <option value="all">Tutte le categorie</option>
          <option v-for="category in categories" :key="category.id" :value="category.id">
            {{ category.name }}
          </option>
        </select>
      </div>
    </div>

    <div v-if="showForm" class="card shadow-lg p-5 mb-5 bg-white rounded-lg border-0 animate__animated animate__fadeIn">
      <h3 class="mb-4 fw-bold text-primary">Crea Nuovo Progetto</h3>
      <form @submit.prevent="createProject" class="form-floating">
        <div class="mb-4">
          <label class="form-label fw-semibold text-secondary">Titolo</label>
          <input type="text" class="form-control form-control-lg rounded-pill" v-model="newProject.title" required>
        </div>
        <div class="mb-4">
          <label class="form-label fw-semibold text-secondary">Descrizione</label>
          <textarea class="form-control rounded-3" rows="4" v-model="newProject.description" required></textarea>
        </div>
        <div class="mb-4">
          <label class="form-label fw-semibold text-secondary">URL Immagine</label>
          <input type="url" class="form-control rounded-pill" v-model="newProject.image">
        </div>
        <div class="mb-4">
          <label class="form-label fw-semibold text-secondary">Categoria</label>
          <select class="form-select rounded-pill" v-model="newProject.category_id" required>
            <option value="">Seleziona una categoria</option>
            <option v-for="category in categories" :key="category.id" :value="category.id">
              {{ category.name }}
            </option>
          </select>
        </div>
        <div class="mb-4">
          <label class="form-label fw-semibold text-secondary">Tecnologie (separate da virgola)</label>
          <input type="text" class="form-control rounded-pill" v-model="newProject.technologies" required>
        </div>
        <button type="submit" class="btn btn-success btn-lg px-5 py-3 rounded-pill shadow-sm hover-lift"
          :disabled="loading">
          <span v-if="loading" class="spinner-border spinner-border-sm me-2" role="status"></span>
          Crea Progetto
        </button>
      </form>
    </div>

    <div v-if="loading && !projects.length" class="text-center my-5">
      <div class="spinner-border text-primary" role="status" style="width: 3rem; height: 3rem;">
        <span class="visually-hidden">Caricamento...</span>
      </div>
    </div>

    <div class="row row-cols-1 row-cols-md-3 g-5">
      <div v-for="project in filteredProjects" :key="project.id" class="col animate__animated animate__fadeIn">
        <div class="card h-100 shadow-lg rounded-4 border-0 hover-lift">
          <div class="position-relative project-image-container">
            <img :src="project.image" class="card-img-top rounded-top-4" :alt="project.title">
            <div class="project-overlay d-flex align-items-center justify-content-center">
              <button class="btn btn-light btn-lg rounded-circle">
                <i class="bi bi-eye"></i>
              </button>
            </div>
            <div class="position-absolute top-0 end-0 m-3">
              <span class="badge bg-light text-dark shadow-sm">{{ project.category_id }}</span>
            </div>
          </div>
          <div class="card-body p-4">
            <h5 class="card-title fw-bold mb-3 text-primary">{{ project.title }}</h5>
            <p class="card-text text-muted mb-4">{{ project.description }}</p>
            <div class="d-flex gap-2 flex-wrap">
              <span v-for="tech in project.technologies" :key="tech"
                class="badge bg-primary bg-opacity-10 text-primary rounded-pill px-3 py-2">
                {{ tech }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
:root {
  --primary-color: #42b883;
  --secondary-color: #35495e;
  --transition-speed: 0.3s;
  --border-radius: 1rem;
  --shadow-sm: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
  --shadow-lg: 0 1rem 3rem rgba(0, 0, 0, 0.175);
}

.hero-section {
  background: linear-gradient(135deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  box-shadow: var(--shadow-sm);
  border-radius: var(--border-radius);
  padding: 3rem 0;
}

.card-img-top {
  height: 250px;
  object-fit: cover;
  transition: transform var(--transition-speed) ease;
}

.project-image-container {
  overflow: hidden;
  position: relative;
  border-radius: var(--border-radius) var(--border-radius) 0 0;
}

.project-overlay {
  position: absolute;
  inset: 0;
  background: rgba(66, 184, 131, 0.9);
  opacity: 0;
  transition: opacity var(--transition-speed) ease;
  display: flex;
  align-items: center;
  justify-content: center;
  backdrop-filter: blur(3px);
}

.project-image-container:hover .project-overlay {
  opacity: 1;
}

.project-image-container:hover .card-img-top {
  transform: scale(1.1);
}

.hover-lift {
  transition: all var(--transition-speed) cubic-bezier(0.4, 0, 0.2, 1);
  will-change: transform;
}

.hover-lift:hover {
  transform: translateY(-8px);
  box-shadow: var(--shadow-lg) !important;
}

.text-gradient {
  background: linear-gradient(45deg, var(--primary-color), var(--secondary-color));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.rounded-lg,
.rounded-4 {
  border-radius: var(--border-radius) !important;
}

.rounded-top-4 {
  border-top-left-radius: var(--border-radius) !important;
  border-top-right-radius: var(--border-radius) !important;
}

.animate__animated {
  animation-duration: 0.8s;
  animation-fill-mode: both;
}

.form-control,
.form-select {
  transition: border-color var(--transition-speed) ease-in-out, box-shadow var(--transition-speed) ease-in-out;
}

.form-control:focus,
.form-select:focus {
  border-color: var(--primary-color);
  box-shadow: 0 0 0 0.25rem rgba(66, 184, 131, 0.25);
  outline: none;
}

.category-filter {
  width: 200px;
  margin: 1rem 0;
}

@media (max-width: 768px) {
  .category-filter {
    width: 100%;
  }

  .card-img-top {
    height: 200px;
  }
}
</style>

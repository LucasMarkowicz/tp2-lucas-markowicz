<script setup>
import { ref, computed } from 'vue'

const personas = [
  { nombre: 'Daniel', apellido: 'Sanchez', correo: 'danielsanchez68@hotmail.com', dni: '20442873' },
  { nombre: 'Juan', apellido: 'Perez', correo: 'j@p.gmail.com', dni: '12345678' },
  { nombre: 'Ana', apellido: 'Suarez', correo: 'a@s.gmail.com', dni: '87654321' },
]

function getNombreCompleto(persona) {
  return `${persona.nombre} ${persona.apellido}`
}

const filtroNombre = ref('')
const filtroDni = ref('')

const personasFiltradas = computed(() => {
  const name = filtroNombre.value.trim()
  const dni = filtroDni.value.trim()

  const useName = name.length >= 3
  const useDni = dni.length >= 3

  return personas.filter((p) => {
    const matchesName = !useName || `${p.nombre} ${p.apellido}`.toLowerCase().includes(name.toLowerCase())
    const matchesDni = !useDni || p.dni.includes(dni)
    return matchesName && matchesDni
  })
})

const showMinCharsAlert = computed(() => {
  const n = filtroNombre.value.trim().length
  const d = filtroDni.value.trim().length
  const nameActiveButShort = n > 0 && n < 3
  const dniActiveButShort = d > 0 && d < 3
  return nameActiveButShort || dniActiveButShort
})
</script>

<template>
  <div class="container mt-4">
    <h1 class="mb-3">Personas</h1>
    <div class="row mb-3">
      <div class="col-md-6 mb-2 mb-md-0">
        <input class="form-control" v-model="filtroNombre" placeholder="Nombre o Apellido (min 3)" />
      </div>
      <div class="col-md-6">
        <input class="form-control" v-model="filtroDni" placeholder="DNI (min 3)" />
      </div>
    </div>
    <div v-if="showMinCharsAlert" class="alert alert-warning py-2" role="alert">
      Ingrese al menos 3 caracteres en el(los) filtro(s) activos.
    </div>
    <div class="card-deck m-0">
      <div class="row">
        <div class="col" v-for="persona in personasFiltradas" :key="persona.dni">
          <div class="card mb-3">
            <div class="card-body">
              <h5 class="card-title">{{ getNombreCompleto(persona) }}</h5>
              <p class="card-text">dni {{ persona.dni }}</p>
              <a :href="`mailto:${persona.correo}`" class="card-link">{{ persona.correo }}</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  
</template>

<style scoped>
.card-title {
  font-weight: 600;
}
</style>

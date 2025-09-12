<template>
  <h1>Rick & Morty API</h1>
  <main class="container mx-auto p-8 grid gap-8">
    <article
      v-for="personaje in personajes"
      :key="personaje.id"
      class="character-card rounded-lg overflow-hidden shadow-lg transform transition-transform duration-300 hover:scale-105"
      @click="showModal(personaje)"
    >
      <div class="imagen-personaje">
        <img :src="personaje.image" :alt="`Imagen de ${personaje.name}`">
      </div>
      <div class="p-6 text-center text-content">
        <h2 class="nombre-personaje">{{ personaje.name }}</h2>
        <hr>
        <p class="estado-personaje">
          <img src="@/assets/icons/svg/bx-pulse.svg" alt="Estado" class="icon">
          {{ personaje.status }}
        </p>
        <p class="especie-personaje">
          <img src="@/assets/icons/svg/bx-user.svg" alt="Especie" class="icon">
          {{ personaje.species }}
        </p>
      </div>
    </article>
  </main>
  <CharacterModal
    v-if="showCharacterDetail"
    :character="selectedCharacter"
    @close="showCharacterDetail = false"
  />
</template>

<script setup>
import { ref, onMounted } from 'vue';
import CharacterModal from './CharacterModal.vue';

const showCharacterDetail = ref(false);
const selectedCharacter = ref(null);
const personajes = ref([]);

function showModal(personaje) {
  selectedCharacter.value = personaje;
  showCharacterDetail.value = true;
}

async function consumoAPI() {
  try {
    const url = 'https://rickandmortyapi.com/api/character';
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error('Error en la solicitud: ' + response.statusText);
    }
    const data = await response.json();
    personajes.value = data.results;
  } catch (error) {
    console.error('Hubo un problema con la solicitud:', error);
  }
}

onMounted(() => {
  consumoAPI();
});
</script>
---

### **Código `style` completo y corregido**

```vue
<style scoped>
h1 {
  text-align: center;
  margin: 30px 0px;
}

/* Contenedor principal del grid */
.container {
  max-width: 80rem;
  margin: 0 auto;
  padding: 1rem;
  display: grid;
  /* Por defecto para móviles, 2 columnas */
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;
}

/* Estilos de las tarjetas, similares a los de carros */
.character-card {
  width: 100%;
  background-color: #ffffff;
  border-radius: 0.5rem;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  overflow: hidden;
  transition: transform 0.3s ease-in-out;
}

.character-card:hover {
  transform: scale(1.03);
}

/* Contenedor de la imagen */
.imagen-personaje {
  width: 100%;
  /* Usa una altura fija para las imágenes, como en la vista de carros */
  height: 200px;
  overflow: hidden;
}

/* Estilos de la imagen para que se adapte al contenedor */
.imagen-personaje img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 10px 10px 0 0;
}

/* Contenedor para el texto */
.text-content {
  padding: 1rem;
  text-align: center;
}

.nombre-personaje {
  font-size: 1.25rem;
  font-weight: bold;
  margin: 0;
  text-align: center;
}

.datos1 {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem;
}

p {
  padding: 0;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  margin: 0;
  text-align: left;
}

.icon {
  width: 14px;
  height: 14px;
}

hr {
  width: 80%;
  margin: 0.5rem auto;
  border: none;
  border-top: 1px solid #ddd;
}

/* Media Query para tablet (a partir de 768px) */
@media (min-width: 768px) {
  .container {
    grid-template-columns: repeat(3, 1fr);
    gap: 2rem;
  }
}

/* Media Query para PC (a partir de 1024px) */
@media (min-width: 1024px) {
  .container {
    grid-template-columns: repeat(4, 1fr);
    gap: 2rem;
  }
}
</style>
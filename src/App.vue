<script setup>
import { ref, computed } from 'vue'
import NavBarComponent from './components/NavBarComponent.vue'
import HomeComponent from './components/pages/HomeComponent.vue'
import AlbumsComponent from './components/pages/AlbumsComponent.vue'
import ArtistsComponent from './components/pages/ArtistsComponent.vue'
import FavoritesComponent from './components/pages/FavoritesComponent.vue'

const routes = {
  '/': HomeComponent,
  '/albums': AlbumsComponent,
  '/artists': ArtistsComponent,
  '/favorites': FavoritesComponent
}

const currentPath = ref(window.location.hash)

window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash
})

const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || '/'] || NotFound
})
</script>
<template>
  <div class="w-screen h-screen flex">
    <NavBarComponent />

    <component :is="currentView" />
  </div>
</template>

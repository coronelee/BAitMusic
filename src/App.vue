<script setup>
import { ref, computed } from 'vue'
import NavBarComponent from './components/NavBarComponent.vue'
import HomeComponent from './components/pages/HomeComponent.vue'
import AlbumsComponent from './components/pages/AlbumsComponent.vue'
import ArtistsComponent from './components/pages/ArtistsComponent.vue'
import FavoritesComponent from './components/pages/FavoritesComponent.vue'
import AuthComponent from './components/pages/AuthComponent.vue'
import RegComponent from './components/pages/RegComponent.vue'
import HeaderComponent from './components/pages/HeaderComponent.vue'
import ProfileComponent from './components/pages/ProfileComponent.vue'

const routes = {
  '/': HomeComponent,
  '/albums': AlbumsComponent,
  '/artists': ArtistsComponent,
  '/favorites': FavoritesComponent,
  '/auth': AuthComponent,
  '/reg': RegComponent,
  '/profile': ProfileComponent
}
const auth = ref(false)
const profile = ref('')
const authed = (profileEmail, status) => {
  profile.value = profileEmail
  status == 201 ? (auth.value = true) : (auth.value = false)
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
  <div class="w-screen h-screen flex font-exo">
    <NavBarComponent />
    <div class="flex flex-col w-full bg-[#26282B]">
      <HeaderComponent :auth="auth" :profile="profile" />
      <component :is="currentView" :authed="authed" :auth="auth" :profile="profile" />
    </div>
  </div>
</template>

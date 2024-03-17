<script setup>
import { ref, computed } from 'vue'
import NavBarComponent from './components/NavBarComponent.vue'
import HomeComponent from './components/pages/HomeComponent.vue'
import AlbumsComponent from './components/pages/AlbumsComponent.vue'
import ArtistsComponent from './components/pages/ArtistsComponent.vue'
import FavoritesComponent from './components/pages/FavoritesComponent.vue'
import AuthComponent from './components/pages/AuthComponent.vue'
import RegComponent from './components/pages/RegComponent.vue'
import HeaderComponent from './components/HeaderComponent.vue'
import ProfileComponent from './components/pages/ProfileComponent.vue'
import PlayerComponent from './components/PlayerComponent.vue'
import NotFound from './components/pages/NotFoundComponent.vue'

const routes = {
  '/': HomeComponent,
  '/albums': AlbumsComponent,
  '/artists': ArtistsComponent,
  '/favorites': FavoritesComponent,
  '/auth': AuthComponent,
  '/reg': RegComponent,
  '/profile': ProfileComponent
}
const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || '/'] || NotFound
})
const currentPath = ref(window.location.hash)

window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash
})

const auth = ref(false)
const profile = ref('')
const player = ref(false)
const playerData = ref([])

const editPlayer = (data, value) => {
  if (value != 'albums') {
    player.value = true
    playerData.value = data
  }
}
const authed = (profileEmail, status) => {
  profile.value = profileEmail
  status == 201 ? (auth.value = true) : (auth.value = false)
}
</script>
<template>
  <div class="w-screen min-h-screen flex font-exo">
    <NavBarComponent />
    <div class="flex flex-col w-full bg-[#26282B]">
      <HeaderComponent :auth="auth" :profile="profile" />
      <component
        :is="currentView"
        :authed="authed"
        :auth="auth"
        :profile="profile"
        :editPlayer="editPlayer"
      />
    </div>
    <PlayerComponent v-if="player" :playerData="playerData" :editPlayer="editPlayer" />
  </div>
</template>

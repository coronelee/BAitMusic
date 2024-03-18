<script setup>
import { ref, computed, onMounted } from 'vue'
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
import axios from 'axios'

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
const playerArt = ref(false)
const editPlayer = (data, value) => {
  if (value != 'albums') {
    playerArt.value = false
    if (value == 'artist') {
      playerArt.value = true
    }
    player.value = true
    playerData.value = data
  }
}
const fullPlayer = ref(false)
const openFullPlayer = () => {
  if (window.screen.width < 980) {
    fullPlayer.value = !fullPlayer.value
  }
}
const authed = (profileEmail, status) => {
  profile.value = profileEmail
  status == 201 ? (auth.value = true) : (auth.value = false)
}

const apiWorking = ref(true)

onMounted(() => {
  axios
    .get('https://ba59ddafd916204d.mokky.dev/albums')
    .then(() => {
      apiWorking.value = true
    })
    .catch(() => {
      apiWorking.value = false
    })
})
</script>
<template>
  <div
    class="min-h-screen flex font-exo overflow-hidden"
    :style="fullPlayer ? 'position: fixed ' : ''"
  >
    <NavBarComponent />
    <div class="flex flex-col w-full bg-[#26282B]">
      <HeaderComponent :auth="auth" :profile="profile" />

      <component
        v-if="apiWorking"
        :is="currentView"
        :authed="authed"
        :auth="auth"
        :profile="profile"
        :editPlayer="editPlayer"
      />
      <span
        v-else
        class="text-white text-center w-full h-1/2 flex flex-col items-center justify-center text-3xl font-exo"
        >API is not working <b class="text-red-500">try later</b></span
      >
    </div>
    <PlayerComponent
      v-if="player"
      :playerData="playerData"
      :editPlayer="editPlayer"
      :playerArt="playerArt"
      :openFullPlayer="openFullPlayer"
      :fullPlayer="fullPlayer"
    />
  </div>
</template>

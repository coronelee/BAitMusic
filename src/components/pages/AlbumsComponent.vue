<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import AlbumComponent from './AlbumComponent.vue'

const props = defineProps({
  editPlayer: Function
})

const album = ref(false)
const dataAlbum = ref([])
const openAlbum = (value, data) => {
  value == 'albums' ? (album.value = true) : (album.value = false)
  dataAlbum.value = data
  console.log(dataAlbum.value)
}

const authors = ref([])
const data = ref([])
onMounted(() => {
  axios.get('https://ba59ddafd916204d.mokky.dev/albums').then((response) => {
    for (let i = 0; i < response.data.length; i++) {
      if (!authors.value.includes(response.data[i].author))
        authors.value.push(response.data[i].author)
    }
    data.value = response.data
  })
})
</script>

<template>
  <div class="w-full h-full font-exo p-12 pt-0 pb-[200px]">
    <img
      v-if="!album"
      src="/globalImages/home/back.svg"
      alt=""
      class="cursor-pointer w-10"
      @click="openAlbum('', '')"
    />
    <div v-if="!album">
      <div class="flex gap-12 mt-4 items-center">
        <img src="/globalImages/albums.jpg" alt="icon" class="w-[250px] h-[250px] rounded-xl" />
        <div class="flex flex-col gap-1 h-full py-10">
          <span class="text-2xl font-bold text-white">Mix albums </span>
          <span class="text-[#7C7C7C] flex gap-4 flex-wrap">
            <span v-for="item in authors.slice(0, 5)" :key="item" class="odd:text-[#7C7C9c]"
              >{{ item }} </span
            >{{ authors.length > 5 ? 'etc...' : '' }}
          </span>
          <span class="text-white/60 text-lg">{{ data.length }} albums</span>
        </div>
      </div>
      <div class="w-full flex flex-col gap-4 mt-4">
        <span
          v-for="item in data"
          :key="item.id"
          class="text-white px-4 text-lg bg-white/5 flex items-center gap-4 rounded-xl justify-between cursor-pointer hover:bg-white/10 transition duration-300"
          @click="openAlbum('albums', item)"
        >
          <div class="flex items-center gap-4">
            <span class="text-center w-[30px]">{{ item.id }}</span>
            <img :src="item.logoSrc" alt="icon" class="w-[50px] h-[50px]" />
            <div class="flex flex-col">
              <span class="text-md font-bold text-white">{{ item.name }}</span>
              <span class="text-[#7C7C7C]">{{ item.author }}</span>
            </div>
          </div>
          <img src="/globalImages/love.svg" alt="" class="w-6" />
        </span>
      </div>
    </div>
    <component
      :is="AlbumComponent"
      :dataAlbum="dataAlbum"
      :openAlbum="openAlbum"
      :editPlayer="props.editPlayer"
      v-else
    />
  </div>
</template>

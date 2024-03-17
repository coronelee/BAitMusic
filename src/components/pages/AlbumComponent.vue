<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'

const props = defineProps({
  dataAlbum: Object,
  openAlbum: Function,
  editPlayer: Function
})

const album = ref([])
const albumAll = ref([])
onMounted(() => {
  axios
    .get(`https://ba59ddafd916204d.mokky.dev/dataAlbums/?idAlbum=${props.dataAlbum.id}`)
    .then((response) => {
      album.value = response.data
      for (let i = 0; i < album.value.length; i++) {
        albumAll.value[i] = album.value[i].id
      }
    })
  console.log(album.value)
})
</script>
<template>
  <div class="w-full h-full font-exo">
    <img
      src="/globalImages/home/back.svg"
      alt=""
      class="cursor-pointer w-10"
      @click="openAlbum('', '')"
    />
    <div class="flex gap-12 mt-4 items-center">
      <img :src="dataAlbum.logoSrc" alt="icon" class="w-[250px] h-[250px] rounded-xl" />
      <div class="flex flex-col gap-1 h-full py-10 max-[600px]:hidden">
        <span class="text-2xl font-bold text-white">{{ dataAlbum.name }}</span>
        <a class="text-[#7C7C7C]">{{ dataAlbum.author }}</a>
        <span class="text-[#7C7C7C]">{{ album.length }} songs</span>
      </div>
    </div>
    <div class="w-full flex flex-col gap-4 mt-4">
      <span
        v-for="item in album"
        :key="item.id"
        class="text-white px-4 text-lg bg-white/5 flex items-center gap-4 rounded-xl justify-between cursor-pointer hover:bg-white/10 transition duration-300"
        @click="editPlayer(item)"
      >
        <div class="flex items-center gap-4">
          <span class="text-center w-[30px]">{{ albumAll.indexOf(item.id) + 1 }}</span>
          <img :src="dataAlbum.logoSrc" alt="icon" class="w-[50px] h-[50px]" />
          <div class="flex flex-col">
            <span class="text-md font-bold text-white">{{ item.name }}</span>
            <span class="text-[#7C7C7C]">{{ dataAlbum.author }}</span>
          </div>
        </div>
        <img src="/globalImages/love.svg" alt="" class="w-6" />
      </span>
    </div>
  </div>
</template>

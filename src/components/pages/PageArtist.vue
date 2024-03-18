<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'

const props = defineProps({
  dataArtist: Object,
  hideArtist: Function,
  editPlayer: Function
})
const dataMusic = ref([])
onMounted(() => {
  document.getElementById('card').style.backgroundImage = `url(${props.dataArtist.imageArtist})`

  loadDataArtist()
})
const loadDataArtist = async () => {
  await axios
    .get(`https://ba59ddafd916204d.mokky.dev/dataAlbums?author=${props.dataArtist.author}`)
    .then((response) => {
      dataMusic.value = response.data
    })
  console.log(dataMusic.value)
}
const maxLength = ref(10)
</script>
<template>
  <div class="w-full h-full font-exo">
    <div
      class="w-full h-[400px] bg-center bg-cover bg-no-repeat rounded-b-3xl flex flex-col p-8 justify-between items-start text-white text-5xl"
      id="card"
    >
      <div
        class="p-2 rounded-full backdrop-blur-3xl hover:bg-white/10 transition cursor-pointer duration-300 hover:-translate-x-1"
        @click="hideArtist()"
      >
        <img src="/globalImages/home/back.svg" alt="" class="w-10" />
      </div>
      <span
        class="backdrop-blur-3xl p-6 rounded-3xl font-bold font-exo max-[600px]:w-full max-[600px]:text-lg text-center"
      >
        {{ dataArtist.author }}</span
      >
    </div>
    <div class="w-full flex flex-col gap-4 mt-4">
      <span
        v-for="item in dataMusic.slice(0, maxLength)"
        :key="item.id"
        class="text-white px-4 py-2 text-lg bg-white/5 flex items-center justify-between gap-4 rounded-xl cursor-pointer hover:bg-white/10 transition duration-300"
        @click="props.editPlayer(item, 'artist')"
      >
        <div class="flex items-center gap-4">
          <img :src="item.logoSrc" alt="icon" class="w-[50px] h-[50px] rounded-xl" />
          <div class="flex flex-col">
            <span class="text-md font-bold text-white">{{ item.name }}</span>
            <span class="text-[#7C7C7C]">{{ dataArtist.author }}</span>
          </div>
        </div>
        <img
          src="/globalImages/love.svg"
          alt=""
          class="w-[30px] h-[30px] rounded-xl max-[450px]:hidden"
        />
      </span>
      <span
        class="text-white px-4 py-2 text-lg bg-white/5 flex items-center justify-center gap-4 rounded-xl cursor-pointer hover:bg-white/10 transition duration-300"
        @click="maxLength += 10 > dataMusic.length ? dataMusic.length : maxLength"
        v-if="maxLength < dataMusic.length"
        >More...</span
      >
    </div>
  </div>
</template>

<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import PageArtist from './PageArtist.vue'

const artistPage = ref(false)

const props = defineProps({
  editPlayer: Function
})

const dataArtist = ref([])
const openArtist = (value, data) => {
  dataArtist.value = data
  artistPage.value = true
}

const hideArtist = () => {
  artistPage.value = false
}

const authors = ref([])
const data = ref([])
const countTrack = ref([])
onMounted(() => {
  axios.get('https://ba59ddafd916204d.mokky.dev/albums').then((response) => {
    for (let i = 0; i < response.data.length; i++) {
      if (!authors.value.includes(response.data[i].author)) {
        authors.value.push(response.data[i].author)
        countTracks(response.data[i].author)
      }
    }
    let unique = Array.from(new Map(response.data.map((item) => [item.author, item])).values())
    data.value = unique
  })
})

const countTracks = (artist) => {
  axios.get(`https://ba59ddafd916204d.mokky.dev/dataAlbums/?author=${artist}`).then((response) => {
    countTrack.value.push(response.data.length)
  })
}
</script>
<template>
  <div class="w-full h-full font-exo p-12 max-[1050px]:p-4 pt-0 pb-[200px]">
    <div v-if="!artistPage">
      <div class="flex gap-12 mt-4 items-center max-[700px]:hidden">
        <img src="/globalImages/micro.jpeg" alt="icon" class="w-[250px] rounded-xl" />
        <div class="flex flex-col gap-1 h-full py-10">
          <span class="text-2xl font-bold text-white"
            >List of <b class="text-red-300">artists</b>
          </span>
          <span class="text-[#7C7C7C] flex gap-4 flex-wrap">
            <span v-for="item in authors.slice(0, 5)" :key="item">{{ item }} </span
            >{{ authors.length > 5 ? 'etc...' : '' }}
          </span>
          <span class="text-white/60 text-lg">{{ data.length }} artists</span>
        </div>
      </div>
      <div class="w-full flex flex-col gap-4 mt-4">
        <span
          v-for="item in data"
          :key="item.id"
          class="text-white pr-4 max-[650px]:pr-0 w-full h-[200px] overflow-hidden text-lg bg-white/5 flex items-center gap-4 rounded-xl justify-between cursor-pointer hover:bg-white/10 transition duration-300"
          @click="openArtist('albums', item)"
        >
          <div class="flex items-center w-full gap-4">
            <div
              alt="icon"
              class="w-[300px] h-[200px] max-[650px]:w-full bg-cover bg-no-repeat bg-center flex justify-center items-center"
              :style="{ backgroundImage: `url(${item.imageArtist})` }"
            >
              <div
                class="flex flex-col min-[650px]:hidden w-full p-6 backdrop-blur-sm rounded-2xl bg-black/10"
              >
                <span class="text-md font-bold text-white">{{ item.author }}</span>
                <span class="text-[#7C7C7C]"
                  >{{ countTrack[authors.indexOf(item.author)] }} tracks</span
                >
              </div>
            </div>
            <div class="flex flex-col max-[650px]:hidden">
              <span class="text-md font-bold text-white">{{ item.author }}</span>
              <span class="text-[#7C7C7C]"
                >{{ countTrack[authors.indexOf(item.author)] }} tracks</span
              >
            </div>
          </div>
          <img src="/globalImages/love.svg" alt="" class="w-6 max-[650px]:hidden" />
        </span>
      </div>
    </div>
    <component
      :is="PageArtist"
      :dataArtist="dataArtist"
      :hideArtist="hideArtist"
      :editPlayer="editPlayer"
      v-else
    />
  </div>
</template>

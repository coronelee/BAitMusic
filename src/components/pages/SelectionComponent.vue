<script setup>
import { onMounted, ref } from 'vue'

const props = defineProps({
  title: String,
  dataAlbums: Array
})
onMounted(() => {
  console.log(props.dataAlbums)
})
let maxLength = ref(5)
</script>
<template>
  <div class="w-full">
    <span class="text-white text-2xl"
      >{{ props.title }}
      <button
        @click="() => (maxLength = maxLength == dataAlbums.length ? 5 : dataAlbums.length)"
        class="text-white/50 border border-white/25 px-4 py-2 ml-8 rounded active:bg-white/50 hover:bg-white/25 transition duration-350"
      >
        {{ maxLength == dataAlbums.length ? 'Show less' : 'Show all' }}
      </button></span
    >
    <div class="flex justify-start flex-wrap gap-8 mt-10">
      <div
        class="p-4 w-[182px] flex flex-col items-center bg-[#1F1F1F] rounded-xl"
        v-for="item in dataAlbums.slice(0, maxLength)"
        :key="item"
      >
        <img src="/iconAlbums/travis-scott-utopia.jpg" alt="" class="rounded-xl h-[150px]" />
        <span class="text-white text-md font-bold">{{ item.name }}</span>
        <span class="text-white/80 text-sm">{{ item.author }}</span>
      </div>
    </div>
  </div>
</template>

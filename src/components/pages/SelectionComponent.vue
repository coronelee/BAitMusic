<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'

const props = defineProps({
  title: String,
  value: String,
  editPlayer: Function,
  openAlbum: Function
})

let maxLength = ref(5)
const data = ref([])

onMounted(() => {
  try {
    axios.get(`https://ba59ddafd916204d.mokky.dev/${props.value}`).then((response) => {
      data.value = response.data
    })
  } catch (err) {
    console.log(err)
  }
})
</script>
<template>
  <div class="w-full">
    <span class="text-white text-2xl"
      >{{ props.title }}
      <button
        @click="() => (maxLength = maxLength == data.length ? 5 : data.length)"
        class="text-white/50 border border-white/25 px-4 py-2 ml-8 rounded active:bg-white/50 hover:bg-white/25 transition duration-350"
      >
        {{ maxLength == data.length ? 'Show less' : 'Show all' }}
      </button></span
    >
    <div class="flex justify-start flex-wrap gap-8 mt-10">
      <div
        class="p-4 w-[182px] flex flex-col items-center bg-[#1F1F1F] rounded-xl hover:bg-white/5 transition duration-500 cursor-pointer"
        v-for="item in data.slice(0, maxLength)"
        :key="item.id"
        @click="props.editPlayer(item, props.value), openAlbum(props.value, item)"
      >
        <img :src="item.logoSrc" alt="" class="rounded-xl h-[150px]" />
        <span class="text-white text-md font-bold text-center mt-4">{{ item.name }}</span>
        <span class="text-white/80 text-sm">{{ item.author }}</span>
      </div>
    </div>
  </div>
</template>

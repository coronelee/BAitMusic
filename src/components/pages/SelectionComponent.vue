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
    <span
      class="text-white text-2xl flex w-full justify-center items-center max-[380px]:justify-center max-[380px]:items-center flex-wrap gap-4"
    >
      <span class="flex max-[380px]:text-lg items-center justify-center">{{ props.title }}</span>
      <button
        @click="() => (maxLength = maxLength == data.length ? 5 : data.length)"
        v-if="data.length > 5"
        class="text-white/50 border border-white/25 px-4 py-2 rounded active:bg-white/50 hover:bg-white/25 transition duration-350"
      >
        {{ maxLength == data.length ? 'Show less' : 'Show all' }}
      </button>
    </span>
    <div class="flex justify-start max-[380px]:justify-center flex-wrap gap-8 mt-10">
      <div
        class="p-4 w-[182px] max-[560px]:w-full flex flex-col items-center bg-[#1F1F1F] rounded-xl hover:bg-white/5 transition duration-500 cursor-pointer"
        v-for="item in data.slice(0, maxLength)"
        :key="item.id"
        @click="props.editPlayer(item, props.value), openAlbum(props.value, item)"
      >
        <img :src="item.logoSrc" alt="" class="rounded-xl w-[150px] max-[560px]:w-full" />
        <span class="text-white text-md font-bold text-center mt-4">{{ item.name }}</span>
        <span class="text-white/80 text-sm">{{ item.author }}</span>
      </div>
    </div>
  </div>
</template>

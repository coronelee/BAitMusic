<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import SelectionComponent from './SelectionComponent.vue'

let dataAlbums = ref([])
let loading = ref(true)
onMounted(() => {
  try {
    loading.value = true

    axios.get('https://ba59ddafd916204d.mokky.dev/albums').then((response) => {
      dataAlbums.value = response.data
      console.log(dataAlbums.value)
    })
  } catch (err) {
    console.log(err)
  } finally {
    loading.value = false
  }
})
</script>
<template>
  <div class="w-full h-full p-12 flex flex-col gap-10">
    <SelectionComponent title="Weekly Top Singles" :dataAlbums="dataAlbums" v-if="!loading" />
    <SelectionComponent title="Weekly Top Singles" :dataAlbums="dataAlbums" v-if="!loading" />
    <!-- <SelectionComponent title="Weekly Top Albums" :items="arr" /> -->
  </div>
</template>

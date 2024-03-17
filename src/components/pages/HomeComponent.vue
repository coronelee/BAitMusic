<script setup>
import { ref } from 'vue'
import SelectionComponent from './SelectionComponent.vue'
import AlbumComponent from './AlbumComponent.vue'
const props = defineProps({
  editPlayer: Function
})

const album = ref(false)
const dataAlbum = ref([])
const openAlbum = (value, data) => {
  value == 'albums' ? (album.value = true) : (album.value = false)
  dataAlbum.value = data
}
</script>
<template>
  <div class="w-full h-full p-12 pt-0 pb-[200px] max-[380px]:p-6">
    <div class="flex flex-col gap-10" v-if="!album">
      <SelectionComponent
        title="Weekly Top Albums"
        value="albums"
        :editPlayer="props.editPlayer"
        :openAlbum="openAlbum"
      />
      <SelectionComponent
        title="Weekly Top Singles"
        value="singles"
        :editPlayer="props.editPlayer"
        :openAlbum="openAlbum"
      />
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

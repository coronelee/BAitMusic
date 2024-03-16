<script setup>
const props = defineProps({
  profile: String
})
import axios from 'axios'
import { onMounted, ref } from 'vue'
const albumData = ref([])
let posUser = ref('user')
onMounted(() => {
  try {
    axios
      .get('https://ba59ddafd916204d.mokky.dev/users?login=' + props.profile)
      .then((response) => {
        posUser.value = response.data[0].position
      })
  } catch (err) {}

  axios.get('https://ba59ddafd916204d.mokky.dev/albums').then((response) => {
    albumData.value.push(response.data)
    // console.log(response.data)
  })
  console.log(albumData.value)
})

const state = ref(0)
const createNewAlbum = () => {
  axios.post('https://ba59ddafd916204d.mokky.dev/albums', {
    name: document.getElementById('albumName').value,
    author: document.getElementById('albumAuthor').value,
    logoSrc: document.getElementById('albumLogo').value
  })
}
</script>

<template>
  <div class="w-full h-full px-12 text-2xl text-white font-exo">
    <div v-if="posUser === 'admin'" class="flex flex-col gap-20">
      <div
        class="flex gap-12 items-start [&>button]:border [&>button]:border-white/25 [&>button]:rounded [&>button]:px-4 [&>button]:py-2 [&>button]:transition [&>button]:duration-700"
      >
        <button @click="state = 0">Add album</button>
        <button @click="state = 1">Edit album</button>
        <button @click="state = 2">Add single</button>
      </div>
      <div
        class="flex flex-col gap-8 w-1/3 [&>input]:h-14 [&>input]:rounded [&>input]:bg-white/5 [&>input]:px-4 [&>input]:py-2 [&>input]:border-b [&>input]:border-white/25 [&>input]:overflow-hidden [&>input]:cursor-pointer [&>input]:text-white [&>input]:outline-none [&>input:focus]:bg-white/15 [&>input]:transition [&>input]:duration-700"
        v-if="state === 0"
      >
        <input type="text" placeholder="Name Album" id="albumName" />
        <input type="text" placeholder="Author" id="albumAuthor" /><input
          type="text"
          placeholder="Logo src"
          value="/iconAlbums/"
          id="albumLogo"
        />
        <button
          class="text-white bg-[#412c3a] text-center border px-4 py-2 rounded active:bg-white/50 hover:bg-white/25 transition duration-350"
          @click="createNewAlbum"
        >
          Create
        </button>
      </div>
      <div
        class="flex flex-col gap-8 w-1/3 [&>input]:h-14 [&>input]:rounded [&>input]:bg-white/5 [&>input]:px-4 [&>input]:py-2 [&>input]:border-b [&>input]:border-white/25 [&>input]:overflow-hidden [&>input]:cursor-pointer [&>input]:text-white [&>input]:outline-none [&>select:focus]:bg-white/15 [&>input]:transition [&>input]:duration-700"
        v-if="state === 1"
      >
        <select
          class="bg-transparent appearance-none h-14 rounded bg-white/5 px-4 py-2 border-b border-white/25 overflow-hidden cursor-pointer text-white outline-none [&>select:focus]:bg-white/15 transition duration-700"
        >
          <option
            v-for="album in albumData[0]"
            :value="album.name"
            :key="album.id"
            class="text-black"
          >
            {{ album.name }}
          </option>
        </select>
        <input type="text" placeholder="New name" />
        <input type="text" placeholder="New author" />
        <button
          class="text-white bg-[#412c3a] text-center border px-4 py-2 rounded active:bg-white/50 hover:bg-white/25 transition duration-350"
        >
          Edit
        </button>
      </div>

      <!--<div
        class="flex flex-col gap-8 w-1/3 [&>input]:h-14 [&>input]:rounded [&>input]:bg-white/5 [&>input]:px-4 [&>input]:py-2 [&>input]:border-b [&>input]:border-white/25 [&>input]:overflow-hidden [&>input]:cursor-pointer [&>input]:text-white [&>input]:outline-none [&>input:focus]:bg-white/15 [&>input]:transition [&>input]:duration-700"
        v-if="state === 2"
      >
        <input type="text" placeholder="Name Album" />
        <input type="text" placeholder="Author" /><input
          type="text"
          placeholder="Logo src"
          value="/iconAlbums/"
        />
        <button
          class="text-white bg-[#412c3a] text-center border px-4 py-2 rounded active:bg-white/50 hover:bg-white/25 transition duration-350"
        >
          Create
        </button>
      </div> -->
    </div>
  </div>
</template>

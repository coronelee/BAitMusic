<script setup>
import { onMounted, ref, watch } from 'vue'
import axios from 'axios'
const props = defineProps({
  playerData: Array,
  editPlayer: Function
})
const pause = ref(false)
const mute = ref(false)
const audio = new Audio()
const currentTime = ref(0)
const durationTime = ref(0)
const maxRange = ref(100)
const volume = ref(1)

onMounted(() => {
  audio.src = props.playerData.src
  audio.play()
  pause.value = true

  currentTime.value = audio.currentTime
  durationTime.value = audio.duration

  audio.onloadedmetadata = function () {
    setInterval(() => {
      let min = String(Number(String(this.currentTime).split('.')[0]) / 60).split('.')[0]
      let sec = Number(String(this.currentTime).split('.')[0]) % 60
      if (sec < 10) {
        sec = '0' + sec
        currentTime.value = min + ':' + sec
      } else currentTime.value = min + ':' + sec

      document.getElementById('timeline').value = this.currentTime
      if (this.currentTime == this.duration) {
        nextPrevMusic(+1)
      }
    }, 1000)

    let minDur = String(Number(String(this.duration).split('.')[0]) / 60).split('.')[0]
    let secDur = Number(String(this.duration).split('.')[0]) % 60
    if (secDur < 10) {
      secDur = '0' + secDur
      durationTime.value = minDur + ':' + secDur
    } else durationTime.value = minDur + ':' + secDur

    maxRange.value = this.duration
  }
})

watch(
  () => props.playerData,
  (newValue) => {
    audio.src = newValue.src
    audio.play()
    document.getElementById('player').classList.remove('hidden')
  }
)

const hidePLayer = () => {
  document.getElementById('player').classList.add('hidden')
}

const nextPrevMusic = (value) => {
  let oldId = [props.playerData.id, props.playerData.idAlbum]
  console.log(oldId)
  if (value > 0) {
    axios
      .get(`https://ba59ddafd916204d.mokky.dev/dataAlbums?idAlbum=${oldId[1]}`)
      .then((response) => {
        if (response.data[response.data.length - 1].id == oldId[0]) {
          props.editPlayer(response.data[0], '')
        } else {
          axios
            .get(
              `https://ba59ddafd916204d.mokky.dev/dataAlbums?idAlbum=${oldId[1]}&id=${oldId[0] + 1}`
            )
            .then((response) => {
              props.editPlayer(...response.data, '')
            })
        }
      })
  } else {
    axios
      .get(`https://ba59ddafd916204d.mokky.dev/dataAlbums?idAlbum=${oldId[1]}`)
      .then((response) => {
        if (response.data[0].id == oldId[0]) {
          props.editPlayer(response.data[response.data.length - 1], '')
        } else {
          axios
            .get(
              `https://ba59ddafd916204d.mokky.dev/dataAlbums?idAlbum=${oldId[1]}&id=${oldId[0] - 1}`
            )
            .then((response) => {
              props.editPlayer(...response.data, '')
            })
        }
      })
  }
}
</script>
<template>
  <div
    class="w-full p-6 fixed bottom-0 h-[150px] bg-[#1F1F1F] flex justify-between gap-8 [&>div]:w-1/3 [&>div]:h-full"
    id="player"
  >
    <div class="flex">
      <img :src="playerData.logoSrc" alt="icon" class="h-full rounded" />
      <div class="flex flex-col gap-1 p-6 text-white justify-center">
        <span class="text-2xl">{{ playerData.name }}</span>
        <span class="text-[#7C7C7C]">{{ playerData.author }}</span>
      </div>
    </div>
    <div class="flex justify-center items-center flex-col text-white font-exo">
      <div class="flex gap-4 [&>img]:w-8 [&>img]:cursor-pointer [&>img]:h-8">
        <img
          src="/globalImages/player/nextPrevMusic.svg"
          alt="prev"
          class="rotate-180"
          @click="() => nextPrevMusic(-1)"
        />
        <img
          :src="pause ? '/globalImages/player/pause.svg' : '/globalImages/player/play.svg'"
          alt="pause"
          @click="() => (audio.paused ? audio.play() : audio.pause(), (pause = !pause))"
        />
        <img
          src="/globalImages/player/nextPrevMusic.svg"
          alt="next"
          @click="() => nextPrevMusic(+1)"
        />
      </div>
      <input
        type="range"
        class="w-full mt-2"
        :max="maxRange"
        defaultValue="0"
        id="timeline"
        @input="audio.currentTime = $event.target.value"
      />
      <div class="flex justify-between w-full">
        <span>{{ currentTime }}</span>
        <span>{{ durationTime }}</span>
      </div>
    </div>
    <div class="flex justify-end items-center gap-4 [&>img]:w-10 [&>img]:cursor-pointer">
      <img
        :src="mute ? '/globalImages/player/volumeoff.svg' : '/globalImages/player/volumeon.svg'"
        alt="mute"
        @click="() => ((mute = !mute), mute ? (audio.volume = 0) : (audio.volume = volume))"
      />
      <input
        type="range"
        class="w-[120px]"
        @input="
          (volume = $event.target.value / 100),
            (audio.volume = volume),
            $event.target.value > 0 ? (mute = false) : (mute = true)
        "
        id="volume"
      />
      <img src="/globalImages/player/hide.svg" alt="hide" @click="() => hidePLayer()" />
    </div>
  </div>
</template>
<style scoped>
input[type='range'] {
  -webkit-appearance: none;
  background: transparent;
}
/* GOOGLE CHROME */

input[type='range']::-webkit-slider-runnable-track {
  height: 10px;
  background: #c5c5c5;
}

input[type='range']::-webkit-slider-thumb {
  -webkit-appearance: none;
  border: none;
  height: 10px;
  width: 10px;
  background: rgb(255, 255, 255);
}
/* MOZILLA FIREFOX */

input[type='range']::-moz-range-thumb {
  -webkit-appearance: none;
  border: none;
  border-radius: 1px;
  height: 6px;
  width: 20px;
  background: rgb(255, 255, 255);
}

input[type='range']::-moz-range-track {
  height: 6px;
  background: rgba(255, 255, 255, 0.3);
}

/* IE */

input[type='range']::-ms-thumb {
  height: 36px;
  width: 16px;
  border-radius: 3px;
  background: #ffffff;
  cursor: pointer;
}
</style>

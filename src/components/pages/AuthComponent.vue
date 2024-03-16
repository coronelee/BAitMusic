<script setup>
import { ref } from 'vue'
import axios from 'axios'
const props = defineProps({
  authed: Function
})
const result = ref('')
const auth = async () => {
  try {
    const response = await axios.post('https://ba59ddafd916204d.mokky.dev/auth', {
      login: document.getElementById('email').value,
      password: document.getElementById('password').value
    })
    if (response.status === 201) {
      props.authed(document.getElementById('email').value, response.status)
      document.location.href = '#/'
    }
  } catch (err) {
    result.value = 'Wrong email or password'
  }
}
</script>
<template>
  <div class="w-full h-full p-12 text-red-400 flex flex-col items-center justify-center gap-4">
    <div
      class="flex gap-4 [&>input]:h-10 [&>input]:rounded [&>input]:h-10 [&>input]:px-4 [&>input]:py-2 [&>input]:rounded [&>input]:bg-white/5 [&>input]:border-b [&>input]:border-white/25 [&>input]:overflow-hidden [&>input]:cursor-pointer [&>input]:text-white [&>input]:outline-none [&>input:focus]:bg-white/15 [&>input]:transition [&>input]:duration-700"
    >
      <input type="text" id="email" placeholder="Email" />
      <input type="text" id="password" placeholder="Password" />
    </div>
    <button
      @click="auth"
      class="bg-white text-[#26282B] hover:bg-[#26282B] hover:text-white px-4 py-2 rounded transition duration-700 border border-white/25"
    >
      Login to your account
    </button>
    {{ result }}
  </div>
</template>

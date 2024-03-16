<script setup>
import { ref } from 'vue'
import axios from 'axios'

const result = ref('')
const register = async () => {
  try {
    const response = await axios.post('https://ba59ddafd916204d.mokky.dev/register', {
      login: document.getElementById('email').value,
      password: document.getElementById('password').value
    })
    if (response.data) {
      document.location.href = '#/auth'
    }
  } catch (err) {
    result.value = 'This email is already in use'
  }
}
</script>
<template>
  <div
    class="w-full h-full bg-[#412c3a] p-12 text-red-400 flex flex-col items-center justify-center gap-4"
  >
    <div
      class="flex gap-4 [&>input]:h-10 [&>input]:rounded [&>input]:h-10 [&>input]:px-4 [&>input]:py-2 [&>input]:rounded [&>input]:bg-white/5 [&>input]:border-b [&>input]:border-white/25 [&>input]:overflow-hidden [&>input]:cursor-pointer [&>input]:text-white [&>input]:outline-none [&>input]:focus:bg-white/15 [&>input]:transition [&>input]:duration-700"
    >
      <input type="text" id="email" placeholder="Email" />
      <input type="text" id="password" placeholder="Password" />
    </div>
    <button
      @click="register"
      class="bg-white text-[#412c3a] hover:bg-[#412c3a] hover:text-white px-4 py-2 rounded transition duration-700 border border-white/25"
    >
      Register new account
    </button>
    {{ result }}
  </div>
</template>

<script setup>
const props = defineProps({
  auth: Boolean,
  profile: String
})
</script>
<template>
  <div class="w-full p-12">
    <div class="flex justify-between">
      <input
        type="text"
        class="w-1/4 bg-[url('/globalImages/home/search.svg')] bg-no-repeat bg-[length:24px] bg-[99%] h-10 px-4 py-2 rounded bg-white/5 border-b border-white/25 overflow-hidden cursor-pointer text-white outline-none focus:bg-white/15 transition duration-700"
        placeholder="Search"
      />
      <div
        class="flex gap-4 text-white [&>a]:border [&>a]:border-white/25 [&>a]:rounded [&>a]:px-4 [&>a]:py-2 [&>a]:transition [&>a]:duration-700"
        v-if="!props.auth"
      >
        <a href="#/auth" class="hover:bg-white hover:text-[#26282B]">Login</a>
        <a href="#/reg" class="bg-white text-[#26282B] hover:bg-transparent hover:text-white"
          >Sign up</a
        >
      </div>
      <a
        v-else
        href="#/profile"
        class="text-white text-md underline flex items-center gap-2 cursor-pointer justify-center cursor-pointer"
      >
        <img src="/globalImages/home/profile.svg" alt="profile" class="w-6" />{{ props.profile }}
      </a>
    </div>
  </div>
</template>

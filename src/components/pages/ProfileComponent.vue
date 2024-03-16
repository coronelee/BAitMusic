<script setup>
const props = defineProps({
  profile: String
})
import AdminPanelComponent from './AdminPanelComponent.vue'
</script>

<template>
  <div class="w-full h-full px-12 text-2xl text-white font-exo">
    <AdminPanelComponent :profile="props.profile" />
  </div>
</template>

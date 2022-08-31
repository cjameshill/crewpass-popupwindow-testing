<script setup>
import { onMounted } from 'vue';
const openWindow = () => {
  window.open('http://127.0.0.1:5174', 'cpVesselPopup', 'width=700,height=800')
}
onMounted(() => {
  window.addEventListener("message", (message) => {
    console.log("message: ", message.data);
  })
})
</script>
<template>
  <main class="w-4/5 flex flex-col justify-center items-center space-y-4">
    <h1 class="text-lg font-medium">Open Popup Window and Message Testing</h1>
    <button @click="openWindow" class="p-4 bg-gray-100 hover:bg-gray-300 rounded-xl">Open Window</button>
  </main>
</template>

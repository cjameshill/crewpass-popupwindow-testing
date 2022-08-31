<script setup>
import { onMounted, ref } from 'vue';
const url = ref("http://127.0.0.1:5174");
const origin = window.location.origin;
const openWindow = () => {
  window.open(`${url.value}?origin=${origin}`, 'cpVesselPopup', 'width=700,height=800')
}
const messages = ref([]);
onMounted(() => {
  console.log("url: ", window.location);
  window.addEventListener("message", (message) => {
    console.log("message: ", message);
    if (message.origin === url.value) {
      messages.value.push(message.data);
    }
  })
})
</script>
<template>
  <main class="w-4/5 flex flex-col justify-center items-center space-y-6">
    <h1 class="text-lg font-medium">Open Popup Window and Message Testing</h1>
    <input v-model="url" type="text" class="p-4 rounded-xl border-2 border-gray-200" placeholder="URL" />
    <button @click="openWindow" class="p-4 bg-gray-100 hover:bg-gray-300 rounded-xl">Open Window</button>
    <h1 class="text-md mt-4">Messages:</h1>
    <pre v-for="message in messages">{{ message }}</pre>
  </main>
</template>

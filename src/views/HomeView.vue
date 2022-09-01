<script setup>
import { onMounted, ref, computed } from 'vue';
const popupUrl = ref("http://127.0.0.1:5173");
const origin = window.location.origin;
const popupFullUrl = computed(() => {
  const url = new URL(popupUrl.value);
  const fullUrl = new URL()
  return url.toString();
})
const openWindow = () => {
  window.open(`${popupUrl.value}?origin=${origin}`, 'cpVesselPopup', 'width=700,height=800')
}
const messages = ref([]);
onMounted(() => {
  console.log("popupUrl: ", window.location);
  window.addEventListener("message", (message) => {
    console.log("message: ", message);
    if (message.origin === popupUrl.value) {
      messages.value.push(message.data);
    }
  })
})
</script>
<template>
  <main class="w-4/5 flex flex-col justify-center items-center space-y-6">
    <h1 class="text-lg font-medium">Open Popup Window and Message Testing</h1>
    <input v-model="popupUrl" type="text" class="p-4 rounded-xl border-2 border-gray-200" placeholder="URL" />
    <button @click="openWindow" class="p-4 bg-gray-100 hover:bg-gray-300 rounded-xl">Open Window</button>
    <pre>{{ popupFullUrl }}</pre>
    <h1 class="text-md mt-4">Messages:</h1>
    <pre v-for="message in messages">{{ message }}</pre>
  </main>
</template>

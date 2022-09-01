<script setup>
import { onMounted, ref, computed } from 'vue';
const popupUrl = ref("https://crewpass-testing-web.netlify.app/crew-messages");
const origin = window.location.origin;
const popupFullUrlObject = computed(() => {
  if (!popupUrl.value) return ""
  const url = new URL(popupUrl.value);
  url.searchParams.append("origin", origin);
  return url;
})
const popupFullUrl = computed(() => {
  if (!popupFullUrlObject.value) return ""
  return popupFullUrlObject.value?.toString();
})
const popupOrigin = computed(() => {
  if (!popupFullUrlObject.value || !popupFullUrlObject.value) return ""
  return popupFullUrlObject.value?.origin;
})
const openWindow = () => {
  window.open(popupFullUrl.value, 'cpVesselPopup', 'width=700,height=800')
}
const messages = ref([]);
onMounted(() => {
  window.addEventListener("message", (message) => {
    if (message.origin === popupOrigin.value) {
      console.log("message: ", message);
      messages.value.push(message.data);
    }
  })
})
</script>
<template>
  <main class="w-4/5 xl:w-1/2 flex flex-col justify-center items-center space-y-6">
    <h1 class="text-lg font-medium">Open Popup Window and Message Testing</h1>
    <input v-model="popupUrl" type="text" class="p-4 w-full rounded-xl border-2 border-gray-200" placeholder="URL" />
    <button @click="openWindow" class="p-4 bg-gray-100 hover:bg-gray-300 rounded-xl">Open Window</button>
    <p>Full URL: {{ popupFullUrl }}</p>
    <p>Origin: {{ popupOrigin }}</p>
    <h1 class="text-md mt-4">Messages:</h1>
    <p class="text-md italic animate-pulse" v-if="messages.length === 0">Listening for messages from: {{ popupOrigin }}
      ...</p>
    <pre v-for="message in messages">{{ message }}</pre>
  </main>
</template>

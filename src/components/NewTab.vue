<template>
  <div>
    <h1>Welcome to My Custom New Tab Page</h1>
    <p>{{ dynamicText }}</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// Store for dynamic text
const dynamicText = ref("This is a simple homepage replacement for the new tab.");

// Load saved text from Chrome storage
onMounted(() => {
  chrome.storage.sync.get(['customText'], (result) => {
    if (result.customText) {
      dynamicText.value = result.customText;
    }
  });

  // Listen for changes in storage and update the text dynamically
  chrome.storage.onChanged.addListener((changes, areaName) => {
    if (areaName === 'sync' && changes.customText) {
      dynamicText.value = changes.customText.newValue;
    }
  });
});
</script>

<style scoped>
/* Add your styling here */
</style>

<template>
  <div>
    <h1>Welcome to My Custom New Tab Page</h1>
    <p>{{ dynamicText }}</p>
    <p>{{ displayDate }}</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// Store for dynamic text
const noYears = ref("This is a simple homepage replacement for the new tab.");
const dob = ref('')



// Load saved text from Chrome storage
onMounted(() => {
  chrome.storage.sync.get(['customText','mainDOB'], (result) => {
    if (result.customText) {
      noYears.value = result.customText;
      dob.value = result.mainDOB;
    }
  });

  // Listen for changes in storage and update the text dynamically
  chrome.storage.onChanged.addListener((changes, areaName) => {
    if (areaName === 'sync' && changes.customText && changes.mainDOB) {
      noYears.value = changes.customText.newValue;
      dob.value = changes.mainDOB.newValue;
    }
  });
});

</script>

<style scoped>
/* Add your styling here */
</style>

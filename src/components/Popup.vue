<template>
  <div>
    <h3>Set New Tab Text</h3>
    <input v-model="inputText" type="text" placeholder="Enter text" />
    <button @click="saveText">Save</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// Reactive variable for input
const inputText = ref("");

// Save the input text to Chrome storage
const saveText = () => {
  chrome.storage.sync.set({ customText: inputText.value }, () => {
    console.log('Custom text saved:', inputText.value);
  });
};
</script>

<style scoped>
/* Add your styling here */
</style>

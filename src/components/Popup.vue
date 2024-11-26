<template>
    <div>
        <h3>Continue timer till</h3>
        <input v-model="inputText" type="number" min="1" max="100" placeholder="years" />

    </div>
    <div class="container">
        <h1>Countdown to {{ inputText }} Years from DOB</h1>
        <label for="dob">Enter your Date of Birth:</label>
        <input type="date" v-model="dobInput">
        <button @click="saveText">Save</button>
        <p>{{ dobInput }}</p>
    </div>
</template>

<script setup>
import { ref } from 'vue'
// Reactive variable for input
const inputText = ref(0);
const dobInput = ref('')

// Save the input text to Chrome storage
const saveText = () => {
    chrome.storage.sync.set({ customNo: inputText.value, mainDOB: dobInput.value }, () => {
        console.log('Custom text saved:', inputText.value);
    });
};
</script>

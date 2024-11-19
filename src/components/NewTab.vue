<template>
  <div>
    <h1>Welcome to My Custom New Tab Page</h1>
    <p>Years {{ years }}, Months {{ months }}, Days {{ days }},weeks {{ weeks }}, Hours {{ hours }}, minutes {{ minutes }}, Seconds {{ seconds }}, Miliseconds {{ miliseconds }} </p>
    <p>{{ messagebox }}</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// Store for dynamic text
const noYears = ref(0);
const dob = ref('')

const newdate = new Date(dob.value)
const targateDate = newdate.setFullYear(newdate.getFullYear() + noYears.value)

//Displaying Data
const years = ref('00')
const months = ref('00')
const weeks = ref('00')
const days = ref('00')
const hours = ref('00')
const minutes = ref('00')
const seconds = ref('00')
const miliseconds = ref('00')
const messagebox = ref('')


// Load saved text from Chrome storage
onMounted(() => {
  chrome.storage.sync.get(['customNo','mainDOB'], (result) => {
    if (result.customText) {
      noYears.value = result.customNo;
      dob.value = result.mainDOB;
    }
  });

  // Listen for changes in storage and update the text dynamically
  chrome.storage.onChanged.addListener((changes, areaName) => {
    if (areaName === 'sync' && changes.customNo && changes.mainDOB) {
      noYears.value = changes.customNo.newValue;
      dob.value = changes.mainDOB.newValue;

    }
  });
});

const updateDate = () => {
  const now = new Date();
  const diff = targateDate - now

  if (diff <= 0) {
      messagebox.value = "Congratulations! You've reached the 50-year mark.";
      return;
    }

    const yrs = Math.floor(diff / (1000 * 60 * 60 * 24 * 365));
    const mon = Math.floor((diff % (1000 * 60 * 60 * 24 * 365)) / (1000 * 60 * 60 * 24 * 30));
    const ds = Math.floor((diff % (1000 * 60 * 60 * 24 * 30)) / (1000 * 60 * 60 * 24));
    const hrs = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    const mins = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
    const secs = Math.floor((diff % (1000 * 60)) / 1000);
    const millis = diff % 1000;

    years.value = yrs;
    months.value = mon;
    days.value = ds;
    hours.value = hrs;
    minutes.value = mins;
    seconds.value = secs;
    miliseconds.value = millis;
}
setInterval(updateDate, 100);
</script>

<style scoped>
/* Add your styling here */
</style>

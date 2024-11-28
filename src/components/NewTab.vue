<template>
    <div>
        <div>
            <h1>Time left:</h1>
            <h1>
                <p>
                    {{ years }} : {{ months }} : {{ days }} : {{ hours }} : {{ minutes }} : {{ seconds }} : {{
                    milliseconds }}
                </p>
            </h1>
            <p>{{ messagebox }}</p>
        </div>
        <div>
            <Shortcut link="https://google.com" name="Google" />
            <Shortcut link="https://codestam.com" name="Codestam" />
            <Shortcut link="https://youtube.com" name="YouTube" />
            <Shortcut link="https://gradus.dev" name="Gradus" />
        </div>
        <div>
            <Profile usr-no="0" />
            <Profile usr-no="1" />
        </div>
        <img src="http://ghchart.rshah.org/0E1033/NishaantKrSingh" alt="Nishant's Github chart" />
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import Shortcut from './comp/Shortcut.vue';
import Profile from './comp/Profile.vue';


// Store for user input
const noYears = ref(0);
const dob = ref('');

// Displaying countdown values
const years = ref('00');
const months = ref('00');
const days = ref('00');
const hours = ref('00');
const minutes = ref('00');
const seconds = ref('00');
const milliseconds = ref('00');
const messagebox = ref('');

// Helper function to validate a date
const isValidDate = (date) => date instanceof Date && !isNaN(date);

// Update countdown logic
const updateDate = () => {
    // Validate DOB and noYears
    if (!dob.value || !noYears.value) {
        messagebox.value = "Please set a valid date of birth and target years.";
        return;
    } else { messagebox.value = '' }

    const parsedDOB = new Date(dob.value);
    if (!isValidDate(parsedDOB)) {
        messagebox.value = "Invalid date of birth provided.";
        return;
    }

    // Calculate target date
    const targetDate = new Date(parsedDOB);
    targetDate.setFullYear(targetDate.getFullYear() + Number(noYears.value));

    // Calculate the difference
    const now = new Date();
    const diff = targetDate - now;

    if (diff <= 0) {
        messagebox.value = "Congratulations! You've reached the target year!";
        return;
    }

    // Calculate time units
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
    milliseconds.value = millis;
};

// Load saved data from Chrome storage
onMounted(() => {
    chrome.storage.sync.get(['customNo', 'mainDOB'], (result) => {
        if (result.customNo && result.mainDOB) {
            noYears.value = result.customNo;
            dob.value = result.mainDOB;
            updateDate();
        }
    });

    // Listen for changes and update dynamically
    chrome.storage.onChanged.addListener((changes, areaName) => {
        if (areaName === 'sync') {
            if (changes.customNo) noYears.value = changes.customNo.newValue;
            if (changes.mainDOB) dob.value = changes.mainDOB.newValue;
            updateDate();
        }
    });
});

setInterval(updateDate, 83);
</script>

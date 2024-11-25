<template>
    <a :href='props.link' target="_blank"><img :src="logo" width='50' height='50'></a>
    <p>{{ props.name }}</p>
</template>

<script setup>
const props = defineProps(['link', 'name'])
const logo = `https://www.google.com/s2/favicons?sz=64&domain=${props.link}`
console.log(logo);

</script>
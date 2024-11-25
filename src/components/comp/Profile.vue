<template>
    <a :href="profile" target="_blank">Profile {{ props.usrNo }}</a> &emsp;
    <a :href="gmail" target="_blank">Gmail {{ props.usrNo }}</a>
    <br>
</template>

<script setup>
const props = defineProps(['usrNo'])
const profile = `https://www.google.com/webhp?authuser=${props.usrNo}`
const gmail = `https://mail.google.com/mail/u/${props.usrNo}/?tab=rm&ogbl#inbox`
</script>
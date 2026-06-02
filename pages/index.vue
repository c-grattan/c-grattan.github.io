<template>
    <p>{{ musing }}</p>
</template>
<script setup lang="ts">
const musing = ref('')
const musings: string[] = [
    'would you like to clean up the growing mound of shit in the public bathroom, or double it and give it to the next guy?',
    'you can\'t make ice cream out of shit, but you can make salad',
    '"the important part of being alive in the big, stupid world is figuring out the world inside of you that is equally big and stupid" - viagra boys',
    'there\'s a party in my head, and I live with the unending feeling that I am late to it',
    'this is my website. you may not like it, but this one is mine'
];
onMounted(() => {
    const randomIndex = Math.floor(Math.random() * musings.length)
    musing.value =  musings[randomIndex];
})
</script>
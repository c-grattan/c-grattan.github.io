<template>
    <NuxtLink v-for="link in links" :to="link.route">{{ link.title }}<br/></NuxtLink>
</template>
<script setup lang="ts">
export type Link = {
    title: string;
    route: string;
};
const props = defineProps<{
    links: Link[]
}>();
</script>
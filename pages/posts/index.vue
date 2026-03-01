<template>
    <p>Posts</p>
    <LinksList :links="posts" />
</template>
<script setup lang="ts">
import type { Link } from '../../components/LinksList.vue';

const posts: Link[] = [
    {
        title: "2025 Year in Review",
        route: "/posts/yearinreview2025"
    },
];
</script>
<template>
    <p>Posts main page</p>
    <LinksList :links="posts" />
</template>
<script setup lang="ts">
import type { Link } from '~/components/LinksList.vue';

const posts: Link[] = [{
    title: "Test post",
    route: "/posts/1"
}];
</script>
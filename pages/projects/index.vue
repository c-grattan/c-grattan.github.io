<template>
    <p>Projects main page</p>
    <LinksList :links="projects" />
</template>
<script setup lang="ts">
import type { Link } from '~/components/LinksList.vue';

const projects: Link[] = [
    {
        title: "Starport",
        route: "https://c-grattan.github.io/probable-octo-guacamole/"
    },
    {
        title: "Droll",
        route: "https://c-grattan.github.io/droll/"
    },
    {
        title: "Haiku Helper",
        route: "https://c-grattan.github.io/HaikuHelper/"
    }
];
</script>
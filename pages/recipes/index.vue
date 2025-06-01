<template>
    <p>Recipes main page</p>
    <LinksList :links="posts" />
</template>
<script setup lang="ts">
import type { Link } from '~/components/LinksList.vue';

const posts: Link[] = [
    {
        title: "Focaccia",
        route: "/recipes/focaccia"
    },
    {
        title: "Granola",
        route: "/recipes/granola"
    },
    {
        title: "Hot Chocolate Powder",
        route: "/recipes/hotchocolatepowder"
    },
    {
        title: "Naan Breads",
        route: "/recipes/naanbread"
    },
    {
        title: "Oat Milk",
        route: "/recipes/oatmilk"
    },
    {
        title: "Pancakes",
        route: "/recipes/pancakes"
    },
];
</script>
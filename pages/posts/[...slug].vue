<template>
    <ContentRenderer :value="post as any" />
</template>
<script setup lang="ts">
const route = useRoute()
const { data: post } = await useAsyncData(route.path, () => {
  return queryCollection('content').path(route.path).first()
});
</script>
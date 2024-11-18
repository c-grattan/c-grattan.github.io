<template>
  <div>
    <p class="h1">Hello, github.io!</p>
    <ThemeToggle :prefererred-theme="prefererredTheme()" />
  </div>
</template>
<script setup lang="ts">
function prefererredTheme(): string {
  return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
}
</script>
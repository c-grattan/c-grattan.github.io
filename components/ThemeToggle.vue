<template>
    <BButton @click="toggleTheme">{{ currentTheme.toUpperCase() }}</BButton>
</template>
<script setup lang="ts">
const props = defineProps({
    prefererredTheme: {
        type: String,
        required: true
    }
});

const currentTheme = ref(props.prefererredTheme);
function setTheme(t: string) {
    document.documentElement.setAttribute("data-bs-theme", t);
}

onBeforeMount(() => {
    setTheme(props.prefererredTheme);
});

function toggleTheme() {
    currentTheme.value = currentTheme.value === 'dark'
    ? 'light'
    : 'dark';
    setTheme(currentTheme.value);
}
</script>
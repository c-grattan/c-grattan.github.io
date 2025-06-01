<template>
    <div class="container">
        <BNavbar toggleable="sm">
        <BNavbarBrand to="/">c-grattan.github.io</BNavbarBrand>
        <BNavbarToggle target="nav-collapse" />
        <BCollapse id="nav-collapse" is-nav>
            <BNavbarNav align="start">
                <BNavItem to="/recipes/">Recipes</BNavItem>
                <BNavItem to="/projects/">Projects</BNavItem>
            </BNavbarNav>
            <BNavbarNav class="w-100" align="end">
                <ThemeToggle :prefererred-theme="prefererredTheme" />
            </BNavbarNav>
        </BCollapse>
        </BNavbar>
        <slot />
    </div>
</template>
<script setup lang="ts">
import { BNavbar } from 'bootstrap-vue-next';
const prefererredTheme = window && window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
</script>
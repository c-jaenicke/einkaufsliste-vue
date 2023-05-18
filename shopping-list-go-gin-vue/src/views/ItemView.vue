<script setup>
import ItemList from '../components/ItemList.vue'
const modeTitle = import.meta.env.VITE_MODE_TITLE
</script>

<template>
    <h2>{{ modeTitle }}</h2>
    <div class="item-list container-fluid">
        <item-list></item-list>
    </div>
</template>

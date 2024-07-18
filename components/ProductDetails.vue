<template>
    <div>
        <h2>{{ product.title }}</h2>
        <img :src="product.image" alt="image thumb" class="thumb">
        <p>Price: ${{ product.price }}</p>
        <p>{{ product.description }}</p>
    </div>
</template>

<script setup lang="ts">
    const { product } = defineProps(['product'])
</script>

<style scoped>
    
</style>
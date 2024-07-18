<template>
    <div>
        <p>{{ error.statusCode }}</p>
        <p>Oooops</p>
        <p>{{ error.message }}</p>
    </div>
</template>

<script setup>
    defineProps(['error'])
</script>

<style scoped>

</style>
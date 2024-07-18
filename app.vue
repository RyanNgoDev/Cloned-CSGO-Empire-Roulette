<template>
	<NuxtLayout>
		<NuxtPage />
	</NuxtLayout>
</template>

<script setup>
	const coins = useState('coins', () => 100);
</script>

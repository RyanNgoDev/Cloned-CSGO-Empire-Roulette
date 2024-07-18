<template>
  <div>
    <header class="shadow-sm">
      <nav class="container mx-auto p-4 flex items-center justify-between">
        <NuxtLink to="/" class="font-bold items-center">Demo</NuxtLink>
        <ul class="flex gap-4 menu">
          <li><NuxtLink to="/">Home</NuxtLink></li>
          <li><NuxtLink to="/about">About</NuxtLink></li>
          <li><NuxtLink to="/products">Products</NuxtLink></li>
        </ul>
        <div class="items-center justify-end"><CoinManager></CoinManager></div>
      </nav>
    </header>
    <div class="container mx-auto p-4">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.router-link-exact-active {
  color: #ddb943;
}

.menu {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
</style>

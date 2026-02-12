<template>
  <div>
    <NuxtRouteAnnouncer />
    <NuxtPage />
  </div>
</template>
<script setup lang="ts">
useHead({
  link: [
    { rel: 'preconnect', href: 'https://fonts.googleapis.com' },
    { rel: 'preconnect', href: 'https://fonts.gstatic.com', crossorigin: '' },
    {
      rel: 'stylesheet',
      href: 'https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700;900&display=swap'
    }
  ]
})
</script>

<style>
html,
body {
  font-family: Roboto, system-ui, -apple-system, "Segoe UI", Arial, sans-serif;
}
</style>
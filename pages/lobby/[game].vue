<!-- /lobby/:game -->

<template>
  <div>
    <p><NuxtLink to="/lobby">Return to Lobby</NuxtLink></p>
    <p>Lobby: {{ game }}</p>
  </div>
</template>

<script setup>
const { game } = useRoute().params
</script>

<style scoped></style>
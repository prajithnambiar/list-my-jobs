<template>
  <button
    @click="toggleColorMode"
    class="p-2 bg-gray-200 dark:bg-gray-800 rounded"
  >
    <span v-if="colorMode.value === 'dark'" class="text-white">🌙 Dark</span>
    <span v-else class="text-black">☀️ Light</span>
  </button>
</template>

<script setup>
const colorMode = useColorMode();

const toggleColorMode = () => {
  colorMode.value = colorMode.value === "dark" ? "light" : "dark";
};
</script>

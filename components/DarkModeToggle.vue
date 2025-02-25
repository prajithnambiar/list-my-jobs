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
// here we have used usColorMode to implement the dark mode feature in our project
const colorMode = useColorMode();

// this below method is called when click in dark mode button. Thus the class changes accordingly
const toggleColorMode = () => {
  colorMode.value = colorMode.value === "dark" ? "light" : "dark";
};
</script>

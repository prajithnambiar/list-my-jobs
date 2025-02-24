<!-- here the file name [] denote a dynamic value -->
<!-- [id].vue fetch the job details based on the id parameter. -->
<template>
  <div
    class="flex items-center justify-center min-h-screen bg-gray-300 dark:bg-gray-900"
  >
    <div class="absolute top-6 right-6">
      <DarkModeToggle />
    </div>
    <div
      class="bg-white dark:bg-gray-800 shadow-lg rounded-lg p-6 w-full max-w-md"
    >
      <div v-if="job">
        <div class="flex items-center mb-4">
          <div>
            <h1 class="text-2xl font-bold text-gray-900 dark:text-white">
              {{ job.title }}
            </h1>
            <p class="text-gray-600 dark:text-gray-300">{{ job.company }}</p>
          </div>
        </div>
        <div class="mb-4">
          <span
            class="bg-blue-100 text-blue-800 text-sm font-semibold mr-2 px-2.5 py-0.5 rounded"
            >{{ job.category }}</span
          >
          <span
            class="bg-green-100 text-green-800 text-sm font-semibold mr-2 px-2.5 py-0.5 rounded"
            >{{ job.salary }}</span
          >
        </div>
        <div class="mt-4">
          <p class="text-gray-700 dark:text-gray-300 leading-relaxed">
            {{ job.description }}
          </p>
        </div>
        <div class="mt-6 text-center">
          <button
            @click="applyForJob"
            class="bg-blue-500 hover:bg-blue-600 text-white font-semibold py-2 px-4 rounded transition duration-300"
          >
            Apply Now
          </button>
        </div>
      </div>
      <div v-else-if="error" class="text-red-500">{{ error.message }}</div>
      <div v-else class="text-gray-500">Loading...</div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";
import DarkModeToggle from "~/components/DarkModeToggle.vue";

const route = useRoute();
// here it references to default value of null
const job = ref(null);
const error = ref(null);

onMounted(async () => {
  try {
    const response = await fetch("/jobs.json");
    if (!response.ok) throw new Error("Failed to fetch jobs data");
    const jobs = await response.json();
    const jobId = parseInt(route.params.id);
    job.value = jobs.find((j) => j.id === jobId);
    if (!job.value) throw new Error("Job not found");
  } catch (err) {
    error.value = err;
  }
});
</script>

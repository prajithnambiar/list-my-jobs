<!-- here the file name [] denote a dynamic value -->
<!-- [id].vue fetch the job details based on the id parameter. -->
<template>
  <div v-if="job">
    <h1 class="text-2xl font-bold mb-4">{{ job.title }}</h1>
    <p class="mb-2"><strong>Company:</strong> {{ job.company }}</p>
    <p class="mb-2"><strong>Category:</strong> {{ job.category }}</p>
    <p class="mb-2"><strong>Salary:</strong> {{ job.salary }}</p>
    <p class="mt-4">{{ job.description }}</p>
  </div>
  <div v-else-if="error">{{ error.message }}</div>
  <div v-else>Loading...</div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";

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

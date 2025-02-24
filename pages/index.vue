<!-- in nuxt.js a a file named index.vue in the pages directory maps to the root route / -->
<template>
  <div class="container mx-auto p-6">
    <header class="flex justify-between items-center mb-6">
      <h1 class="text-3xl font-bold mb-6 text-center">Job Listings</h1>
      <DarkModeToggle />
    </header>
    <!-- category filter button display -->
    <div class="flex justify-center mb-6">
      <button
        v-for="category in categories"
        :key="category"
        @click="selectedCategory = category"
        :class="[
          'px-4 py-2 m-1 rounded',
          selectedCategory === category
            ? 'bg-blue-500 text-white'
            : 'bg-gray-200 text-gray-700',
        ]"
      >
        {{ category }}
      </button>
      <button
        @click="selectedCategory = 'All'"
        :class="[
          'px-4 py-2 m-1 rounded',
          selectedCategory === 'All'
            ? 'bg-blue-500 text-white'
            : 'bg-gray-200 text-gray-700',
        ]"
      >
        All
      </button>
    </div>

    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
      <!-- Job is displayed according to the filters applied -->
      <div
        v-for="job in filteredJobs"
        :key="job.id"
        class="bg-white shadow-lg p-6 rounded-lg hover:shadow-custom transition-shadow"
      >
        <h2 class="text-xl font-semibold text-gray-800">{{ job.title }}</h2>
        <p class="text-gray-600">{{ job.company }}</p>
        <p class="text-sm font-medium text-blue-500">{{ job.category }}</p>
        <!-- <NuxtLink> helps to points to the dynamic route /jobs/[id], allowing users to click and view the detailed job description. -->
        <p class="text-gray-700">{{ job.salary }}</p>
        <NuxtLink
          :to="`/jobs/${job.id}`"
          class="text-blue-600 font-medium mt-2 inline-block"
        >
          View Details →
        </NuxtLink>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, onMounted, computed } from "vue";
import DarkModeToggle from "~/components/DarkModeToggle.vue";

// ref function is used to create reactive references to values. Here it redferences to an empty array. This array will store the job listings
const jobs = ref([]);
const selectedCategory = ref("All");
const categories = ref(["Frontend", "Backend", "Full Stack"]); // Defined the categories

// filter data logic
const filteredJobs = computed(() => {
  if (selectedCategory.value === "All") {
    return jobs.value;
  }
  return jobs.value.filter((job) => job.category === selectedCategory.value);
});

// as soon as the component is onMounted, this method runs and we fetch the data from the api to be displayed on the landing page
onMounted(async () => {
  const response = await fetch("/jobs.json");
  jobs.value = await response.json();
});
</script>

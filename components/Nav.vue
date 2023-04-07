<template>
  <nav>
    <div
      :class="['folderPath', gradientClasses[index % gradientClasses.length]]"
      v-for="(group, groupName, index) in groupedRoutes"
      :key="groupName"
    >
      <h2>{{ groupName }}</h2>
      <ul>
        <button
          v-for="route in group"
          :key="route.name"
          class="rounded-md bg-indigo-600 px-3.5 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
        >
          <nuxt-link :to="route.path">{{ route.name }}</nuxt-link>
        </button>
      </ul>
    </div>
  </nav>
</template>

<script setup>
import { computed } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const groupedRoutes = computed(() => {
  const grouped = {};

  router.options.routes.forEach((route) => {
    if (route.path !== "*") {
      const groupName = route.path.split("/")[1] || "root";

      if (!grouped[groupName]) {
        grouped[groupName] = [];
      }

      grouped[groupName].push(route);
    }
  });

  return grouped;
});

const gradientClasses = [
  "bg-gradient-to-r from-blue-500 via-teal-400 to-green-500",
  "bg-gradient-to-r from-red-500 via-pink-400 to-purple-500",
  "bg-gradient-to-r from-yellow-500 via-orange-400 to-red-500",
  "bg-gradient-to-r from-green-500 via-blue-400 to-indigo-500",
  "bg-gradient-to-r from-pink-500 via-purple-400 to-pink-500",
  "bg-gradient-to-r from-orange-500 via-yellow-400 to-orange-500",
  // Add more gradient classes here if needed
];
</script>

<style scoped>
nav {
  @apply bg-gradient-to-r from-gray-800 via-gray-900 to-black py-12;
}
.folderPath {
  display: grid;
  border-radius: 30px;
  width: 70vw;
  max-width: 1200px;
  place-content: center;
  align-items: flex-start;
  text-align: center;
  margin: 0 auto;
  margin-bottom: 2rem;
  padding: 2em;
}
button {
  margin: 0.5em;
  @apply text-xl;
}
h2 {
  @apply text-4xl font-bold;

  margin-bottom: 1em;
}
ul {
  display: grid;
  place-content: center;
  margin: 0 auto;
  grid-template-columns: auto auto auto auto;
}
</style>

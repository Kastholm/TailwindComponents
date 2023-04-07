<template>
  <nav>
    <div
      class="folderPath"
      v-for="(group, groupName) in groupedRoutes"
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
</script>

<style scoped>
.folderPath {
  background: rgba(214, 190, 9, 0.703);
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-bottom: 2rem;
  padding: 2em;
}
button {
  margin: 0.5em;
}
h2 {
  @apply text-2xl;
  margin-bottom: 1em;
}
ul {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
}
</style>

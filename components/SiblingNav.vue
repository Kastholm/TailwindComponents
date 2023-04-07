<template>
  <nav>
    <div class="siblingNav" :class="navVisible ? 'h-fit' : 'h-16'">
      <button
        type="button"
        class="toggleButton rounded-full absolute -top-8 left-[49%] w-fit bg-indigo-600 p-2 text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
        @click="navVisible = !navVisible"
      >
        <PlusIcon
          v-if="!navVisible"
          class="h-10 w-10 m-auto"
          aria-hidden="true"
        />
        <MinusSmallIcon v-else class="h-10 w-10 m-auto" aria-hidden="true" />
      </button>
      <nuxt-link to="/">
        <button
          type="button"
          class="inline-flex items-center gap-x-2 rounded-md bg-indigo-600 px-3.5 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 mb-4 mx-auto mt-1"
        >
          Hjem
          <ArrowLeftCircleIcon class="-mr-0.5 h-8 w-12" aria-hidden="true" />
        </button>
      </nuxt-link>
      <div class="buttonRow">
        <span v-for="sibling in siblingRoutes" :key="sibling.name">
          <nuxt-link :to="sibling.path">
            <button
              class="sibling rounded-md bg-indigo-600 px-3.5 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
            >
              {{ sibling.name }}
            </button>
          </nuxt-link>
        </span>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { computed, ref } from "vue";
import { useRouter, useRoute } from "vue-router";
import {
  ArrowLeftCircleIcon,
  PlusIcon,
  MinusSmallIcon,
} from "@heroicons/vue/20/solid";
const router = useRouter();
const route = useRoute();

const siblingRoutes = computed(() => {
  const folderPath = route.path.split("/").slice(0, -1).join("/");
  return router.options.routes.filter(
    (r) => r.path !== "*" && r.path.startsWith(folderPath)
  );
});

const navVisible = ref(false);
</script>

<style scoped>
.siblingNav {
  @apply fixed  bottom-0 left-0 right-0 w-screen bg-gradient-to-r from-gray-800 via-gray-900 to-black pb-12;
}
.buttonRow {
  @apply grid place-content-center place-items-center grid-cols-3 gap-4 max-w-[1000px] m-auto;
}
.sibling {
  @apply text-xl w-fit;
}
.router-link-active button {
  background: #000;
  @apply text-xl;
}
</style>

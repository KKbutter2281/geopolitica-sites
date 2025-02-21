<script setup>
const route = useRoute();
const slug = route.params.slug || [];
const path = Array.isArray(slug) ? `/teams/${slug.join("/")}` : `/teams/${slug}`;

const { data: page } = await useAsyncData(`team-${slug.join("-")}`, () =>
  queryContent(path).findOne()
);

// Set the page title dynamically
useHead({
  title: page.value?.title || "Faction Not Found",
});
</script>

<template>
  <div v-if="page" :style="{ backgroundColor: page.color, height: '100vh', display: 'flex', alignItems: 'center', justifyContent: 'center' }">
    <h1>{{ page.title }}</h1>
  </div>
  <div v-else>
    <h1>Page Not Found</h1>
  </div>
</template>

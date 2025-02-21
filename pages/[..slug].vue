<script setup>
import { useRoute, useHead } from '#imports'

// Get the slug from the URL (it will be an array, e.g. ['teams', 'blackfang'] for /teams/blackfang)
const route = useRoute()
const slug = route.params.slug || []

// Determine the content path:
// If no slug is provided, assume the homepage (content/index.md)
// Otherwise, join the slug with "/" to form the path in the content directory.
const contentPath = slug.length ? `/${slug.join('/')}` : '/index'

// Fetch the content using Nuxt Content
const { data: page } = await useAsyncData(`page-${slug.join('-')}`, () =>
  queryContent(contentPath).findOne()
)

// Set the browser page title based on the content metadata
useHead({
  title: page.value?.title || "Page Not Found"
})
</script>

<template>
  <div v-if="page" :style="{ backgroundColor: page.color, height: '100vh', display: 'flex', alignItems: 'center', justifyContent: 'center' }">
    <h1>{{ page.title }}</h1>
  </div>
  <div v-else>
    <h1>Page Not Found</h1>
  </div>
</template>

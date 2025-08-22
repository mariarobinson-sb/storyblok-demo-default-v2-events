<script setup>
defineProps({ blok: Object, index: Number });
const gridClasses = getGridClasses();
</script>

<template>
  <section
    v-editable="blok"
    class="page-section image-text-section relative bg-white"
  >
    <div class="container ">
      <!-- <div class="mx-auto mb-12 max-w-2xl text-center">
          <CategoriesList v-if="blok?.categories?.length" :categories="blok.categories" class="mt-12 flex justify-center space-x-4" />
      </div> -->
      <div>{{ blok.categories.length }}</div>

      <ul v-if="blok.categories.length" :class="gridClasses">
        <li v-for="category in blok.categories" :key="category._uid" class="max-w-sm rounded-lg bg-primary-background p-6 xl:max-w-none xl:p-12">
          <div class="mt-8 flex items-center gap-4">
            <!-- <div class="aspect-square size-16 shrink-0 overflow-hidden rounded-full bg-white"><img v-if="category?.content?.icon?.filename" :src="getOptimizedImage(category.content.icon, 128, 128)" :alt="category.content.icon?.alt" width="64" height="64" /></div> -->
            <div>
              <p class="font-black">Category: {{ category.index }}</p>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </section>
</template>

      <!-- <div v-if="blok.description" class="prose prose-lg mb-6">
        <StoryblokRichText :doc="blok.description" />
      </div>
      <div class="mx-auto mb-12 max-w-2xl text-center">
          <CategoriesList v-if="blok?.categories?.length" :categories="blok.categories" class="mt-12 flex justify-center space-x-4" />
      </div> -->

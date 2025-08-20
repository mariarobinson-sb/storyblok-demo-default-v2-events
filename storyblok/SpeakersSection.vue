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
      <div v-if="blok.description" class="prose prose-lg mb-6">
        <StoryblokRichText :doc="blok.description" />
      </div>
      <ul v-if="blok.speakers.length" :class="gridClasses">
        <li v-for="speaker in blok.speakers" :key="speaker._uid" class="max-w-sm rounded-lg bg-primary-background p-6 xl:max-w-none xl:p-12">
          <div class="mt-8 flex items-center gap-4">
            <div class="aspect-square size-16 shrink-0 overflow-hidden rounded-full bg-white"><img v-if="speaker?.content?.headshot?.filename" :src="getOptimizedImage(speaker.content.headshot, 128, 128)" :alt="speaker.content.headshot?.alt" width="64" height="64" /></div>
            <div>
              <p class="font-black">{{ speaker?.content?.name }}</p>
              <p>{{ speaker?.content?.title }}</p>
              <p>{{ speaker?.content?.company }}</p>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </section>
</template>

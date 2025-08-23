<script setup>
import { Swiper, SwiperSlide } from 'swiper/vue';
import { Autoplay, Navigation } from 'swiper/modules';
import 'swiper/css';
import 'swiper/css/navigation';

defineProps({ blok: Object });

const modules = [Autoplay, Navigation];
</script>

<template>
  <Swiper
    v-editable="blok"
    :slides-per-view="1"
    :space-between="10"
    :navigation="true"
    :autoplay="true"
    :auto-height="true"
    :modules="modules"
    :loop="true"
    class="swiper"
  >
    <SwiperSlide v-for="banner in blok.banners" :key="banner.uuid">
      <Banner
        v-if="banner.content"
        :blok="banner.content"
        :referenced="true"
      />
    </SwiperSlide>
  </Swiper>
</template>

<style scoped>
.swiper {
  --swiper-theme-color: var(--medium);
}
</style>

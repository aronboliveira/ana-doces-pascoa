<template>
  <div class="carousel-item" :class="{ active: isActive }">
    <img
      :src="`/carousel/slide_${suffix}.webp`"
      class="d-block w-100"
      :alt="`Menu de Páscoa Slide ${suffix}`"
      loading="lazy"
    />
  </div>
</template>
<script>
export default {
  name: "CarouselItem",
  props: {
    suffix: {
      type: String,
      required: true,
    },
    isActive: {
      type: Boolean,
      default: false,
    },
  },
};
</script>
<style scoped>
.carousel-item {
  transition: transform 0.6s ease-in-out;
  height: 100%;
  text-align: center;
}

.carousel-item img {
  max-height: 100%;
  max-width: 100%;
  margin: 0 auto;
}
</style>

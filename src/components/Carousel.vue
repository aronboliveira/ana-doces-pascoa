<script>
import CarouselItem from "./CarouselItem.vue";
import { onMounted } from "vue";
export default {
  name: "Carousel",
  components: { CarouselItem },
  setup() {
    onMounted(() => {
      if (typeof bootstrap !== "undefined") {
        new bootstrap.Carousel(document.getElementById("carouselMenu"), {
          ride: "carousel",
        });
      }
    });
  },
};
</script>
<template>
  <div id="carouselMenu" class="carousel slide" data-bs-ride="carousel">
    <div class="carousel-indicators">
      <button
        v-for="i in 9"
        :key="'indicator-' + i"
        type="button"
        data-bs-target="#carouselMenu"
        :data-bs-slide-to="i - 1"
        :class="{ active: i === 1 }"
        :aria-label="'Slide ' + i"
      ></button>
    </div>
    <div class="carousel-inner">
      <CarouselItem
        v-for="i in 9"
        :key="'slide-' + i"
        :is-active="i === 1"
        :suffix="i.toString()"
      />
    </div>
    <button
      class="carousel-control-prev"
      type="button"
      data-bs-target="#carouselMenu"
      data-bs-slide="prev"
    >
      <span class="carousel-control-prev-icon" aria-hidden="true"></span>
      <span class="visually-hidden">Anterior</span>
    </button>
    <button
      class="carousel-control-next"
      type="button"
      data-bs-target="#carouselMenu"
      data-bs-slide="next"
    >
      <span class="carousel-control-next-icon" aria-hidden="true"></span>
      <span class="visually-hidden">Próximo</span>
    </button>
  </div>
</template>
<style>
#carouselMenu {
  width: 100%;
  max-width: 75vw;
  margin: auto auto;
  aspect-ratio: 16/9;
}

#app {
  max-width: 100%;
  max-height: 100%;
}

.carousel-inner {
  position: relative;
  width: 100%;
  overflow: hidden;
  border-radius: 0.5rem;
  box-shadow: 0 0.25rem 0.5rem rgba(0, 0, 0, 0.1);
}

.carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  background-color: #f8f9fa;
}
</style>

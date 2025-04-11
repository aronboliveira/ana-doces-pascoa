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
      (() => {
        const queryForImgs = () => {
            const ci = document.querySelector(".carousel-inner");
            if (!ci?.isConnected) return { ci, imgs: [] };
            const imgs = ci.getElementsByTagName("img");
            if (!imgs.length) return { ci, imgs: [] };
            return { ci, imgs };
          },
          { ci, imgs } = queryForImgs();
        if (!imgs.length) return;
        setInterval(() => {
          const maxHeight = Math.max(
            ...Array.from(imgs)
              .map((img) =>
                parseFloat(getComputedStyle(img).height.replace("px", ""))
              )
              .filter((n) => !isNaN(n) && Number.isFinite(n))
          );
          if (maxHeight < 300) return;
          [
            ...["next", "prev"].map((b) =>
              ci.closest(".carousel")?.querySelector(`.carousel-control-${b}`)
            ),
          ].forEach((b) => {
            if (!b?.isConnected) return;
            b.style.height = `${Math.round(maxHeight)}px`;
          });
          const indicators = ci
            .closest(".carousel")
            ?.querySelector(".carousel-indicators");
          if (!(indicators instanceof HTMLElement)) return;
          indicators.style.top = `${Math.round(maxHeight) * 1.1}px`;
          const carouselInnerRect = ci.getBoundingClientRect(),
            carouselBottom = carouselInnerRect.bottom + scrollY,
            indicatorsRect = indicators.getBoundingClientRect(),
            indicatorsTop = indicatorsRect.top + scrollY,
            limit = carouselBottom * 0.78;
          if (indicatorsTop < limit) indicators.style.opacity = "0";
          else indicators.style.opacity = "1";
          const rt = document.getElementById("app");
          if (!rt) return;
          rt.style.height = `${Math.round(maxHeight) * 1.3}px`;
        }, 200);
      })();
    });
  },
};
</script>
<template>
  <div id="carouselMenu" class="carousel slide" data-bs-ride="carousel">
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
  </div>
</template>
<style>
#carouselMenu {
  width: 100%;
  max-width: 75vw;
  margin: auto auto;
  aspect-ratio: 16/9;
  transform: translateY(-0.25rem);
  box-shadow: 0.5rem 0.5rem 0.5rem #0004;
}

#app {
  max-width: 100%;
  max-height: 100%;
}

.carousel-inner {
  position: relative;
  width: 100%;
  overflow: hidden;
  border: ridge 0.5rem #fff5;
  border-radius: 0.5rem;
  box-shadow: 0 0.25rem 0.5rem rgba(0, 0, 0, 0.1);
}

.carousel-item {
  max-height: 100vh;
  max-width: 100%;
  max-height: 100%;
  overflow: auto;
  scrollbar-width: none;
  border: none;
  box-shadow: 0 0.25rem 0.5rem rgba(0, 0, 0, 0.1);
}

.carousel-item img {
  max-width: 100%;
  max-height: 100%;
  width: 100%;
  height: 100%;
  overflow: auto;
  object-fit: contain;
  background-color: #f5f0f5;
}

@media (min-width: 850px) {
  [data-indicator="slide__1"],
  [data-indicator="slide__6"] {
    object-position: 0 -3vh;
  }
  [data-indicator="slide__7"],
  [data-indicator="slide__8"],
  [data-indicator="slide__9"] {
    object-position: 0 -21vh;
  }
}

.carousel-control-next-icon,
.carousel-control-prev-icon {
  max-height: 100%;
  filter: invert(0.5);
  overflow: hidden;
}

.carousel-indicators {
  position: absolute;
  max-height: 10%;
  max-width: 100%;
  margin-block: 0;
  filter: invert(0.5);
}
</style>

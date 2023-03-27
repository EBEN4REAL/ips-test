<template>
  <div class="ips-carousel-mockup-wrapper position-relative">
    <img
      src="@/assets/images/FE-test-assets.svg"
      class="ips-camera-pointers img-fluid"
    />
    <div class="ambient-bg" :style="{ boxShadow: currentSlide.shadow }"></div>

    <img src="@/assets/images/iPhone-mokup.png" class="iphone-mockup" />
    <div
      v-for="(slider, index) in slides"
      :key="index"
      :class="`slide-${index} ips-slider-photo`"
    >
      <img
        :src="getPic(slider.largeImg)"
        :alt="slide"
        :class="[`slide-${index} ips-img`]"
      />
    </div>
  </div>
</template>

<script>
import { ref, watch } from "vue";

export default {
  props: {
    slides: {
      type: Array,
      default: () => [],
      required: true,
    },
    previousSlideIndex: {
      type: Array,
      default: 0,
      required: Number,
    },
  },

  setup({ slides }) {
    const getPic = (src) => {
      return require(`@/assets/images/${src}`);
    };

    const getPrevSlidePic = (src) => {
      return require(`@/assets/images/${src}`);
    };

    const currentSlide = ref(slides[0]);

    console.log("currentSlide", currentSlide.value);

    return {
      getPic,
      currentSlide,
      getPrevSlidePic,
    };
  },
};
</script>

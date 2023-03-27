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
import { ref } from "vue";

export default {
  props: {
    slides: {
      type: Array,
      default: () => [],
      required: true,
    },
    previousSlideIndex: {
      type: Number,
      default: 0,
      required: true,
    },
  },

  setup(props) {
    const getPic = (src) => {
      return require(`@/assets/images/${src}`);
    };

    const getPrevSlidePic = (src) => {
      return require(`@/assets/images/${src}`);
    };

    const currentSlide = ref(props.slides[0]);

    return {
      getPic,
      currentSlide,
      getPrevSlidePic,
    };
  },
};
</script>

<style>
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>
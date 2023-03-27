<template>
  <div class="ips-mockup-slide-wrapper">
    <div style=" ">
      <div class="progress--container mt-3">
        <div
          class="progress-bar"
          :class="[`progress-bar-${index}`]"
          v-for="(slide, index) in sliders"
          :key="index"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    sliders: {
      type: Array,
      default: () => [],
      required: true,
    },
  },
};
</script>
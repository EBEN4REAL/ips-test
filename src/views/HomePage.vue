
<template>
  <div>
    <!-- Header section -->
    <header class="app-container">
      <nav class="navbar">
        <div>
          <img src="@/assets/images/ips-logo.svg" />
        </div>
      </nav>
    </header>

    <!-- Waiting list section  -->
    <div class="app-container wait-list-wrapper row">
      <div class="wait-list-form-wrapper col-md-6 col-sm-12 col-xs-12 col-lg-6">
        <div class="wait-list-badge">New course</div>
        <div>
          <h2 class="wait-list-header">iPhone Photo Academy</h2>
          <p class="wait-list-para">
            Sign up now to get notified <br />
            when this course is available!
          </p>
        </div>
        <div class="wait-list-form">
          <form @submit.prevent="submitForm">
            <base-input
              v-model:modelValue="email"
              type="text"
              label="Enmail"
              :formError="formError"
            />
            <ips-button
              @submit-form="submitForm"
              :formError="formError"
              type="submit"
            >
              Please Notify Me
            </ips-button>
          </form>
        </div>
      </div>
      <div
        class="position-relative col-md-6 col-md-6 col-sm-12 col-xs-12 col-lg-6"
        :class="[deviceWidth >= 820 ? 'ips-carousel-wrapper': '']"
      >
        <img
          class="ips-img-small"
          :src="
            getPrevSlidePic(
              sliders?.[previousSlideIndex - 1]?.smallImg ||
                'carousel-photo-02.jpg'
            )
          "
        />
        <caraousel-sliders
          :slides="carouselSlider"
          @get-pic="getPic"
          :key="() => key++"
        />

        <timeline-sliders :sliders="sliders" />
      </div>
    </div>
  </div>
</template>

<script>
import { toRefs, reactive, watch, onMounted, computed, ref } from "vue";
import { useRouter } from "vue-router";
import { gsap } from "gsap";
import BaseInput from "@/components/BaseInput.vue";
import IpsButton from "@/components/IpsButton.vue";
import CaraouselSliders from "@/components/CaraouselSliders.vue";
import TimelineSliders from "@/components/TimelineSliders.vue";
import { sliders } from "@/constants";

export default {
  components: {
    BaseInput,
    IpsButton,
    CaraouselSliders,
    TimelineSliders,
  },
  setup() {
    const state = reactive({
      email: "",
      formError: "",
      formValid: false,

      sliders,
      activeIndex: 0,
      key: 0,
    });

    const router = useRouter();

    const previousSlideIndex = computed(() => {
      return state.activeIndex - 1 < 0
        ? state.sliders.length - 1
        : state.activeIndex - 1;
    });

    const carouselSlider = computed(() => {
      return state.sliders.filter((slider) => !!slider.active);
    });

    const validateEmail = () => {
      const isValid = /\S+@\S+\.\S+/.test(state.email);
      if (!state.email) {
        state.formError = "Enter your Email Address";
        state.formValid = false;
      } else if (!isValid) {
        state.formError = "Please enter a valid email";
        state.formValid = false;
      } else {
        state.formError = "";
        state.formValid = true;
      }
    };

    watch(() => state.email, validateEmail);

    const submitForm = () => {
      validateEmail();
      if (!state.formValid) {
        return;
      }
      router.push({ path: "/success-page", query: { email: state.email } });
    };

    const playSlideshow = () => {
      state.sliders.forEach((slider, index) => {
        if (index === state.activeIndex) {
          slider.active = true;
        } else {
          slider.active = false;
        }
      });

      gsap.to(`.slide-${state.activeIndex}`, {
        duration: 0,
        opacity: 1,
        onComplete: () => {
          state.activeIndex = (state.activeIndex + 1) % state.sliders.length;
        },
      });
      gsap.to(".progress", {
        duration: 8,
        onComplete: () => {
          // gsap.set(".progress", { width: 0 });
          playSlideshow();
        },
      });

      // gsap.to(`.slide-${state.activeIndex}`, {
      //   duration: 8,
      //   onComplete: () => {
      //     state.activeIndex = (state.activeIndex + 1) % state.sliders.length;
      //     playSlideshow();
      //   },
      // });
    };

    const getPrevSlidePic = (src) => {
      return require(`@/assets/images/${src}`);
    };

    const deviceWidth = ref(screen.width)

    const getDeviceWidth = () => {
      let screenWidth = window.innerWidth;

      window.addEventListener("resize", function () {
        let newScreenWidth = window.innerWidth;

        if (newScreenWidth !== screenWidth) {
          screenWidth = newScreenWidth;
          deviceWidth.value = screenWidth
        }
      });
    };

    onMounted(() => {
      getDeviceWidth()
      playSlideshow();

      const progress = Array.from(document.querySelectorAll(".progress-bar"));

      const playNext = (e) => {
        const current = e && e.target;
        let next;

        if (current) {
          const currentIndex = progress.indexOf(current);
          if (currentIndex < progress.length) {
            next = progress[currentIndex + 1];
          }
          current.classList.remove("active");
          current.classList.add("passed");
        }

        if (!next) {
          progress.map((el) => {
            el.classList.remove("active");
            el.classList.remove("passed");
          });
          next = progress[0];
        }
        next.classList.add("active");
      };

      progress.map((el) => {
        el.addEventListener("animationend", playNext, false);
      });

      playNext();
    });

    return {
      submitForm,
      getPrevSlidePic,
      carouselSlider,
      previousSlideIndex,
      deviceWidth,
      ...toRefs(state),
    };
  },
};
</script>




<script>
export default {
  CONFIG: {
    ladderWidth: 6060,
    maxWrapperWidth: 500,
    stepWidth: 60,
    defaultStepOffset: 3.3,
  },
  name: "hawaii-ladder",
  props: {
    step: {
      type: Number,
      default: 1,
      required: false,
    },
  },
  data() {
    return {
      isAnimationFrameScheduled: false,
      leftScroll: 0,
      offset: 0,
      thumbRatio: 0,
      thumbPosition: 0,
    };
  },
  methods: {
    initThumbRatio() {
      const thumbWrapperWidth = this.$refs.thumb?.parentElement.offsetWidth;
      const interceptorWidth = this.$refs.interceptor?.offsetWidth;
      const thumbWidth = this.$refs.thumb?.offsetWidth;
      const ladderWidthDiff =
        this.$options.CONFIG.ladderWidth - interceptorWidth;
      console.log((thumbWrapperWidth - thumbWidth) / ladderWidthDiff);
      this.thumbRatio = (thumbWrapperWidth - thumbWidth) / ladderWidthDiff;
    },
    initScrollingController() {
      const interceptor = document.querySelector(".interceptor");
      const currentLocation = document.querySelector(
        ".scrollbar-current-location"
      );

      interceptor.scrollLeft =
        0 +
        (this.step - this.$options.CONFIG.defaultStepOffset) *
          this.$options.CONFIG.stepWidth;

      currentLocation.style.left =
        this.step + 5 < 10
          ? `5%`
          : this.step + 5 > 90
          ? `95%`
          : `${this.step + 1}%`;
    },
    updateScroll() {
      const sync = () => {
        const scrollLeft = this.$refs.interceptor.scrollLeft;
        if (scrollLeft !== this.leftScroll) {
          this.$refs.wrapper.scrollLeft = scrollLeft;
          this.$refs.wrapper.scrollTop =
            this.$options.CONFIG.ladderWidth - this.offset - scrollLeft;
          this.thumbPosition = Math.floor(scrollLeft * this.thumbRatio);
        }
        this.isAnimationFrameScheduled = false;
      };

      if (!this.isAnimationFrameScheduled) {
        this.isAnimationFrameScheduled = true;
        requestAnimationFrame(sync);
      }
    },
    initWrapperScrollPosition() {
      this.offset = Math.min(
        this.$refs.wrapper.offsetWidth,
        this.$options.CONFIG.maxWrapperWidth
      );
      this.$refs.wrapper.scrollTop =
        this.$options.CONFIG.ladderWidth -
        this.offset -
        (this.step - this.$options.CONFIG.defaultStepOffset) *
          this.$options.CONFIG.stepWidth;
      this.$refs.wrapper.scrollLeft =
        0 +
        (this.step - this.$options.CONFIG.defaultStepOffset) *
          this.$options.CONFIG.stepWidth;
    },
  },
  mounted() {
    this.initThumbRatio();
    this.initWrapperScrollPosition();
    this.initScrollingController();
  },
};
</script>

<template>
  <div class="m-0 h-screen bg-[rgb(49,46,38)] p-4">
    <div
      class="relative max-h-[750px] w-full max-w-[500px] overflow-hidden rounded-[20px] bg-white pb-7"
    >
      <img
        src="https://static.tildacdn.com/tild6664-3536-4231-b664-666530313930/Group_29.svg"
        class="absolute left-6 top-6"
      />
      <div class="ladder relative mt-8 aspect-square w-full max-w-[500px]">
        <div class="corner-gradient absolute right-0 top-0 z-50"></div>
        <div class="bottom-gradient absolute bottom-0 z-50"></div>
        <div
          ref="interceptor"
          @scroll="updateScroll"
          class="interceptor absolute z-50 h-full w-full overflow-y-hidden"
        >
          <div class="h-2 w-[6060px]"></div>
        </div>
        <div ref="wrapper" class="wrapper -mr-4 h-full w-full overflow-auto">
          <div class="content relative h-[6060px] w-[6060px]">
            <svg
              fill="none"
              class="ladder-fill absolute bottom-0 left-0 h-full w-full"
            >
              <path
                class="base duration-300"
                stroke=""
                stroke-width="0"
                stroke-linejoin="round"
                fill="#F588593D"
                :d="`M2 ${$options.CONFIG.ladderWidth} ${Array.from(
                  { length: step - 1 },
                  (_, i) => i + 1
                ).map(
                  (x) =>
                    `v-${$options.CONFIG.stepWidth} h${$options.CONFIG.stepWidth}`
                )} v-60 H6060 V6060`"
              />
              <path
                class="line"
                stroke=""
                stroke-width="0"
                stroke-linejoin="round"
                fill="url(#fff)"
                :d="`M${step * $options.CONFIG.stepWidth} ${
                  $options.CONFIG.ladderWidth - step * $options.CONFIG.stepWidth
                } h700 v-30 h-700`"
              />
              <defs>
                <linearGradient id="fff" gradientTransform="rotate(90)">
                  <stop offset="0%" stop-color="#ffffff00" />
                  <stop offset="50%" stop-color="#ffffff00" />
                  <stop offset="100%" stop-color="#F588593D" />
                </linearGradient>
              </defs>
            </svg>

            <svg fill="none" class="absolute bottom-0 left-0 h-full w-full">
              <path
                stroke="url(#paint0_linear_1887_3134)"
                stroke-width="4"
                stroke-linejoin="round"
                d="M2 6060
  
                v-55 q 0 -5 5 -5 h50 q 5 0 5 -5
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5
  
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5
  
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5
  
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5
  
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5
  
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5
  
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5
  
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5
  
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5
  
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50 q 5 0 5 -5 
                v-50 q 0 -5 5 -5 h50
  
                h60 
                "
              />
              <defs>
                <linearGradient
                  id="paint0_linear_1887_3134"
                  x2="0%"
                  y2="0%"
                  x1="100%"
                  y1="0%"
                  gradientUnits="userSpaceOnUse"
                  gradientTransform="rotate(90)"
                >
                  <stop offset="0%" stop-color="#F58859" />
                  <stop
                    class="ladder-gradient-start"
                    stop-color="#F58859"
                    :offset="`${step * 0.99}%`"
                  />
                  <stop
                    class="ladder-gradient-end"
                    stop-color="#EDEDED"
                    :offset="`${step * 0.99 + 0.4}%`"
                  />
                  <stop offset="100%" stop-color="#EDEDED" />
                </linearGradient>
              </defs>
            </svg>

            <div
              class="user-marker absolute h-[40px] w-[40px] rounded-full"
              :style="{
                left: `calc(${step * 0.99 - 1}% + 12px)`,
                bottom: `calc(${step * 0.99}% + 10px)`,
              }"
            >
              <slot></slot>
            </div>
            <div class="checkpoints relative h-full w-full">
              <template v-for="(item, index) in new Array(100).fill(0)">
                <div
                  :style="{
                    left: `calc(${index * 0.99}% + 2px)`,
                    bottom:
                      (index + 1) % 5 !== 0
                        ? `calc(${index * 0.99}% + 27px)`
                        : index + 1 !== step
                        ? `calc(${index * 0.99}% + 70px)`
                        : '',
                  }"
                  :class="{
                    checkpoint: true,
                    'checkpoint--flag': index + (1 % 5) === 0,
                    'checkpoint--current': index + 1 === step,
                    'checkpoint--completed': index + 1 < step,
                  }"
                >
                  <div v-if="(index + 1) % 5 === 0" class="checkpoint--flag">
                    <svg
                      width="24"
                      height="31"
                      viewBox="0 0 24 31"
                      v-if="index + 1 !== step"
                    >
                      <rect
                        x="4.8075"
                        y="0.49707"
                        width="2.15508"
                        height="30.5027"
                        rx="1.07754"
                      />
                      <path
                        d="M22.0101 8.5905C22.7891 8.94658 22.7891 10.0534 22.0101 10.4095L7.41561 17.0802C6.75332 17.3829 5.99991 16.8989 5.99991 16.1707L5.99991 2.82933C5.99991 2.10114 6.75332 1.61712 7.41561 1.91983L22.0101 8.5905Z"
                      />
                    </svg>
                  </div>
                  <div v-else>{{ index + 1 }}</div>
                </div>
                <div
                  v-if="(index + 1) % 5 === 0"
                  :class="{
                    checkpoint: true,
                    'checkpoint--flag': true,
                    'checkpoint--current': index + 1 === step,
                    'checkpoint--completed': index + 1 < step,
                  }"
                  :style="{
                    left: `calc(${index * 0.99}% + 2px)`,
                    bottom: `calc(${index * 0.99}% + 22px)`,
                  }"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="30"
                    height="30"
                    viewBox="0 0 1024 1024"
                  >
                    <path
                      d="M1024 320.496c0-35.344-28.654-64-63.998-64H850.754c28.272-27.888 46.368-64.447 46.368-109.472 0-55.44-31.84-115.664-121.216-115.664-117.6 0-215.84 125.216-262 195.408-46.192-70.176-147.44-195.392-265.024-195.392-89.376 0-121.216 60.224-121.216 115.664 0 45.008 18.592 81.584 47.44 109.472H64.002c-35.344 0-64 28.656-64 64V512.08h64.56v416.56c0 35.344 28.655 64 64 64h767.68c35.343 0 64-28.656 64-64V512.064h63.76V320.496zM775.906 95.376c39.568 0 57.216 16.625 57.216 51.665 0 71.088-79.344 109.439-153.968 109.439H570.818c45.471-67.536 125.504-161.104 205.088-161.104zm-527.025.001c79.6 0 162.655 93.568 208.127 161.088H348.64c-74.624 0-156.976-39.344-156.976-110.432 0-35.024 17.648-50.656 57.217-50.656zm711.12 352.687h-416V320.496h416v127.568zm-896-127.568h416v127.568h-416zm64.56 191.568h351.44v416.56h-351.44zm767.696 416.56H544.001v-416.56h352.256v416.56z"
                      style="stroke-width: 100px"
                    />
                  </svg>
                </div>
              </template>
            </div>
          </div>
        </div>
      </div>
      <div
        class="relative mx-auto mt-5 h-2 w-[95%] rounded-full bg-[#EDEDED]"
        :style="{ height: '8px' }"
      >
        <div
          ref="thumb"
          :style="{ transform: `translateX(${this.thumbPosition}px)` }"
          class="scrollbar-thumb h-full w-10 rounded-full bg-[#B4B4C8]"
        ></div>
        <div
          class="scrollbar-current-location absolute top-0 z-10 h-full w-2 rounded-full bg-[#F58859]"
        ></div>
      </div>
    </div>
  </div>
</template>

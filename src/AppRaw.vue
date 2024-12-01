<script>
const CONFIG = {
  ladderWidth: 6060,
  maxWrapperWidth: 500,
  stepWidth: 60,
  defaultStepOffset: 3.3,
};

export default {
  name: "AppRaw",
  props: {
    step: {
      type: Number,
      required: false,
      default: 23,
    },
  },
  methods: {
    initScrollingController(offset, step) {
      const wrapper = document.querySelector(".wrapper");
      const thumb = document.querySelector(".scrollbar-thumb");
      const interceptor = document.querySelector(".interceptor");
      const currentLocation = document.querySelector(
        ".scrollbar-current-location"
      );
      const thumbWrapperWidth = thumb.parentElement.offsetWidth;
      const interceptorWidth = interceptor.offsetWidth;
      const thumbWidth = thumb.offsetWidth;

      const ladderWidthDiff = CONFIG.ladderWidth - interceptorWidth;
      const thumbRatio = (thumbWrapperWidth - thumbWidth) / ladderWidthDiff;

      interceptor.scrollLeft =
        0 + (step - CONFIG.defaultStepOffset) * CONFIG.stepWidth;
      currentLocation.style.left =
        step + 5 < 10 ? `5%` : step + 5 > 90 ? `95%` : `${step + 1}%`;

      let scheduledAnimationFrame = false;
      let lastScrollLeft = 0;

      const sync = () => {
        const scrollLeft = interceptor.scrollLeft;

        if (scrollLeft !== lastScrollLeft) {
          wrapper.scrollLeft = scrollLeft;
          wrapper.scrollTop = CONFIG.ladderWidth - offset - scrollLeft;

          const thumbPosition = Math.floor(scrollLeft * thumbRatio);
          thumb.style.transform = `translateX(${thumbPosition}px)`;

          lastScrollLeft = scrollLeft;
        }

        scheduledAnimationFrame = false;
      };

      interceptor.addEventListener("scroll", () => {
        if (!scheduledAnimationFrame) {
          scheduledAnimationFrame = true;
          requestAnimationFrame(sync);
        }
      });
    },
    renderCheckpoints(step) {
      const checkpoints = document.querySelector(".checkpoints");

      const flagSvg = `
            <svg width="24" height="31" viewBox="0 0 24 31">
              <rect x="4.8075" y="0.49707" width="2.15508" height="30.5027" rx="1.07754" />
              <path d="M22.0101 8.5905C22.7891 8.94658 22.7891 10.0534 22.0101 10.4095L7.41561 17.0802C6.75332 17.3829 5.99991 16.8989 5.99991 16.1707L5.99991 2.82933C5.99991 2.10114 6.75332 1.61712 7.41561 1.91983L22.0101 8.5905Z" />
            </svg>`;

      const presentSvg = `
            <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 1024 1024">
              <path d="M1024 320.496c0-35.344-28.654-64-63.998-64H850.754c28.272-27.888 46.368-64.447 46.368-109.472 0-55.44-31.84-115.664-121.216-115.664-117.6 0-215.84 125.216-262 195.408-46.192-70.176-147.44-195.392-265.024-195.392-89.376 0-121.216 60.224-121.216 115.664 0 45.008 18.592 81.584 47.44 109.472H64.002c-35.344 0-64 28.656-64 64V512.08h64.56v416.56c0 35.344 28.655 64 64 64h767.68c35.343 0 64-28.656 64-64V512.064h63.76V320.496zM775.906 95.376c39.568 0 57.216 16.625 57.216 51.665 0 71.088-79.344 109.439-153.968 109.439H570.818c45.471-67.536 125.504-161.104 205.088-161.104zm-527.025.001c79.6 0 162.655 93.568 208.127 161.088H348.64c-74.624 0-156.976-39.344-156.976-110.432 0-35.024 17.648-50.656 57.217-50.656zm711.12 352.687h-416V320.496h416v127.568zm-896-127.568h416v127.568h-416zm64.56 191.568h351.44v416.56h-351.44zm767.696 416.56H544.001v-416.56h352.256v416.56z" style="&#10;    stroke-width: 100px;&#10;"/>
            </svg>`;

      for (let index = 0; index < 100; index += 1) {
        const element = document.createElement("div");

        element.classList.add("checkpoint");
        element.style.left = `calc(${index * 0.99}% + 2px)`;

        if ((index + 1) % 5 === 0) {
          if (index + 1 !== step) {
            element.classList.add("checkpoint--flag");
            element.style.bottom = `calc(${index * 0.99}% + 70px)`;
            element.innerHTML = flagSvg;
          }

          const present = document.createElement("div");
          present.classList.add("checkpoint", "checkpoint--flag");
          present.style.left = `calc(${index * 0.99}% + 2px)`;
          present.style.bottom = `calc(${index * 0.99}% + 22px)`;
          present.innerHTML = presentSvg;
          checkpoints.append(present);
          if (index + 1 === step) present.classList.add("checkpoint--current");
          if (index + 1 < step) present.classList.add("checkpoint--completed");
        } else {
          element.textContent = index + 1;
          element.style.bottom = `calc(${index * 0.99}% + 27px)`;
        }

        if (index + 1 === step) element.classList.add("checkpoint--current");
        if (index + 1 < step) element.classList.add("checkpoint--completed");
        checkpoints.append(element);
      }
    },
    renderUserMarker(step) {
      const userMarker = document.querySelector(".user-marker");
      userMarker.style.bottom = `calc(${step * 0.99}% + 10px)`;
      userMarker.style.left = `calc(${step * 0.99 - 1}% + 12px)`;
    },
    renderLadderGradient(step) {
      const start = document.querySelector(".ladder-gradient-start");
      const end = document.querySelector(".ladder-gradient-end");
      start.setAttribute("offset", `${step * 0.99}%`);
      end.setAttribute("offset", `${step * 0.99 + 0.4}%`);
    },
    renderLadderFill(step) {
      const basePath = document.querySelector(".ladder-fill .base");
      const linePath = document.querySelector(".ladder-fill .line");
      // REFERENCE: 'M2 6060 v-60 h60 v-60 h60 v-60 h60 v-60 h60 v-60 h60 v-60 h60 v-60 h60 v-60 h60 v-60 h60 v-60 h60 v-60 H6060 V6060'
      // M2 6060 - start; v-60 h60 - step; v-60 H6060 V6060 - finish
      const basePathDefinition = `M2 ${CONFIG.ladderWidth} ${Array.from(
        { length: step - 1 },
        (_, i) => i + 1
      ).map(
        (x) => `v-${CONFIG.stepWidth} h${CONFIG.stepWidth}`
      )} v-60 H6060 V6060`;
      const linePathDefinition = `M${step * CONFIG.stepWidth} ${
        CONFIG.ladderWidth - step * CONFIG.stepWidth
      } h700 v-30 h-700`;
      basePath.setAttribute("d", basePathDefinition);
      linePath.setAttribute("d", linePathDefinition);
    },
    initWrapperScrollPosition(step) {
      const wrapper = document.querySelector(".wrapper");
      const offset = Math.min(wrapper.offsetWidth, CONFIG.maxWrapperWidth);
      wrapper.scrollTop =
        CONFIG.ladderWidth -
        offset -
        (step - CONFIG.defaultStepOffset) * CONFIG.stepWidth;
      wrapper.scrollLeft =
        0 + (step - CONFIG.defaultStepOffset) * CONFIG.stepWidth;
      return offset;
    },
  },
  mounted() {
    const STEP = this.step;
    const offset = this.initWrapperScrollPosition(STEP);
    this.initScrollingController(offset, STEP);
    this.renderCheckpoints(STEP);
    this.renderUserMarker(STEP);
    this.renderLadderGradient(STEP);
    this.renderLadderFill(STEP);
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
      <meta
        name="viewport"
        content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"
      />
      <div class="ladder relative mt-8 aspect-square w-full max-w-[500px]">
        <div class="corner-gradient absolute right-0 top-0 z-50"></div>
        <div class="bottom-gradient absolute bottom-0 z-50"></div>
        <div class="interceptor absolute z-50 h-full w-full overflow-y-hidden">
          <div class="h-2 w-[6060px]"></div>
        </div>
        <div class="wrapper -mr-4 h-full w-full overflow-auto">
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
              />
              <path
                class="line"
                stroke=""
                stroke-width="0"
                stroke-linejoin="round"
                fill="url(#fff)"
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
                  <stop class="ladder-gradient-start" stop-color="#F58859" />
                  <stop class="ladder-gradient-end" stop-color="#EDEDED" />
                  <stop offset="100%" stop-color="#EDEDED" />
                </linearGradient>
              </defs>
            </svg>

            <div
              class="user-marker absolute h-[40px] w-[40px] rounded-full bg-cover"
            >
              <slot></slot>
            </div>
            <div class="checkpoints relative h-full w-full"></div>
          </div>
        </div>
      </div>
      <div class="relative mx-auto mt-5 h-2 w-[95%] rounded-full bg-[#EDEDED]">
        <div
          class="scrollbar-thumb h-full w-10 rounded-full bg-[#B4B4C8]"
        ></div>
        <div
          class="scrollbar-current-location absolute top-0 z-10 h-2 w-2 rounded-full bg-[#F58859]"
        ></div>
      </div>
    </div>
  </div>
</template>

<!-- <style scoped>
.interceptor,
.wrapper {
  scrollbar-width: none;
  -ms-overflow-style: none;
}
.wrapper {
  will-change: scroll-top scroll-left;
}
.wrapper::-webkit-scrollbar {
  display: none;
}
.user-marker {
  background-image: url("https://static.tildacdn.com/tild3336-6432-4463-b434-373731326266/ghfghfgh.jpg");
  background-position: center;
  background-size: contain;
}
.checkpoints {
  font-size: 20px;
  font-weight: 600;
  color: #b4b4c8;
  text-align: center;
  font-family: "PT Sans Caption", sans-serif;
}
.checkpoint {
  position: absolute;
  width: 60px;
}
.checkpoint--flag {
  display: flex;
  justify-content: center;
  fill: #b4b4c8;
}
.checkpoint--completed {
  color: #f1a38c;
  fill: #f1a38c;
}
.checkpoint--current {
  color: #e8653f;
  fill: #e8653f;
  font-weight: 700;
}
.corner-gradient {
  width: 50%;
  height: 30px;
  background-image: linear-gradient(
    to bottom,
    rgb(255, 255, 255) 0%,
    rgb(255, 255, 255) 30%,
    rgba(0, 0, 0, 0) 100%
  );
}
.bottom-gradient {
  width: 100%;
  height: 30px;
  background-image: linear-gradient(
    to top,
    rgb(255, 255, 255) 0%,
    rgb(255, 255, 255) 15%,
    rgba(0, 0, 0, 0) 100%
  );
}
.scrollbar-thumb {
  will-change: transform;
}
</style> -->

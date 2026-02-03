<template>
  <div class="h-[calc(100%-80px)] w-full overflow-hidden">
    <div class="flex flex-col h-full bg-[#F4F4F4]">
      <HeaderComp />

      <div class="flex-1 px-4 py-2 relative">
        <div
          v-if="isLoading"
          class="w-full h-full flex items-center justify-center flex-col"
        >
          <self-building-square-spinner
            :animation-duration="6000"
            :size="40"
            color="#8fe507"
          />
        </div>
        <div v-else-if="currentStage" class="w-full h-full relative">
          <div class="relative w-full h-full">
            <template
              v-for="(stage, stackIndex) in displayStack"
              :key="stage.id"
            >
              <div
                v-if="stackIndex === 0"
                class="absolute inset-0 z-30 rounded-3xl overflow-hidden shadow-xl bg-white border border-gray-200 flex flex-col"
                :style="stackCardStyle(stackIndex)"
                @touchstart.passive="onTouchStart"
                @touchmove.passive="onTouchMove"
                @touchend.passive="onTouchEnd"
              >
                <div class="h-[65%] w-full relative bg-gray-300">
                  <img :src="stage.image" class="w-full h-full object-cover" />
                  <div
                    class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent"
                  ></div>
                </div>

                <div
                  class="flex-1 p-5 flex flex-col justify-center bg-white relative -mt-4 rounded-t-3xl z-10"
                >
                  <h2 class="font-oswald font-bold text-3xl mb-1 text-black">
                    {{ stage.company }}
                  </h2>
                  <p
                    class="font-inter font-semibold text-gray-500 text-lg mb-4"
                  >
                    {{ stage.title }}
                  </p>

                  <div class="flex flex-wrap gap-2">
                    <span
                      v-for="(tag, index) in stage.tags"
                      :key="index"
                      class="px-4 py-1 bg-gray-200 rounded-full font-inter text-sm font-medium text-gray-800"
                      >{{ tag }}</span
                    >
                  </div>

                  <div
                    class="mt-4 flex items-center text-gray-500 font-inter text-sm"
                  >
                    <svg
                      class="mr-1"
                      xmlns="http://www.w3.org/2000/svg"
                      width="16"
                      height="16"
                      viewBox="0 0 24 24"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                    >
                      <path
                        d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z"
                      />
                      <circle cx="12" cy="10" r="3" />
                    </svg>
                    {{ stage.location }}
                  </div>
                </div>

                <!-- Top card overlay indicators -->
                <div class="absolute inset-0 pointer-events-none z-40">
                  <div
                    v-if="swipeDelta < 0"
                    class="absolute left-6 top-1/2 -translate-y-1/2 flex items-center gap-3 px-6 py-4 rounded-lg bg-red-600/95 text-white font-extrabold text-3xl shadow-2xl border-4 border-white"
                    :style="{
                      opacity: swipeOpacity,
                      transform: 'translateY(-50%) rotate(-8deg)',
                    }"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      class="w-8 h-8"
                      viewBox="0 0 24 24"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2"
                    >
                      <path d="M18 6L6 18" />
                      <path d="M6 6l12 12" />
                    </svg>
                    NOPE
                  </div>
                  <div
                    v-if="swipeDelta > 0"
                    class="absolute right-6 top-1/2 -translate-y-1/2 flex items-center gap-3 px-6 py-4 rounded-lg bg-emerald-600/95 text-white font-extrabold text-3xl shadow-2xl border-4 border-white"
                    :style="{
                      opacity: swipeOpacity,
                      transform: 'translateY(-50%) rotate(8deg)',
                    }"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      class="w-8 h-8"
                      viewBox="0 0 24 24"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2"
                    >
                      <path d="M20 6L9 17l-5-5" />
                    </svg>
                    LIKE
                  </div>
                </div>
              </div>

              <div
                v-else
                class="absolute inset-0 z-20 rounded-3xl overflow-hidden bg-white border border-gray-200"
                :style="stackCardStyle(stackIndex)"
              >
                <div class="h-[65%] w-full relative bg-gray-300">
                  <img :src="stage.image" class="w-full h-full object-cover" />
                  <div
                    class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent"
                  ></div>
                </div>
                <div
                  class="flex-1 p-5 flex flex-col justify-center bg-white relative -mt-4 rounded-t-3xl z-10"
                >
                  <h2 class="font-oswald font-bold text-3xl mb-1 text-black">
                    {{ stage.company }}
                  </h2>
                  <p
                    class="font-inter font-semibold text-gray-500 text-lg mb-4"
                  >
                    {{ stage.title }}
                  </p>
                </div>
              </div>
            </template>
          </div>
        </div>
        <div
          v-else
          class="flex h-full items-center justify-center text-center p-8"
        >
          <div>
            <h3 class="font-oswald text-2xl mb-2">Helemaal bij!</h3>
            <p class="text-gray-500">Er zijn geen nieuwe stages meer.</p>
          </div>
        </div>
      </div>

      <div class="h-24 px-8 flex items-center justify-between pb-4">
        <button
          @click="nextStage"
          @mousedown.prevent="startHold('left')"
          @mouseup="stopHold"
          @mouseleave="stopHold"
          @touchstart.prevent="startHold('left')"
          @touchend="stopHold"
          @touchcancel="stopHold"
          class="w-16 h-16 rounded-full bg-white border-2 border-gray-200 flex items-center justify-center text-black shadow-lg hover:scale-110 transition-transform"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="30"
            height="30"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path d="M17 14V2" />
            <path
              d="M9 18.12 10 14H4.17a2 2 0 0 1-1.92-2.56l2.33-8A2 2 0 0 1 6.5 2H19a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2h-2.76a2 2 0 0 0-1.79 1.11L12 22h0a3.13 3.13 0 0 1-3-3.88Z"
            />
          </svg>
        </button>

        <button
          class="w-12 h-12 rounded-full bg-black flex items-center justify-center text-white shadow-lg hover:scale-110 transition-transform"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <circle cx="12" cy="12" r="10" />
            <path d="M12 16v-4" />
            <path d="M12 8h.01" />
          </svg>
        </button>

        <button
          @click="nextStage"
          @mousedown.prevent="startHold('right')"
          @mouseup="stopHold"
          @mouseleave="stopHold"
          @touchstart.prevent="startHold('right')"
          @touchend="stopHold"
          @touchcancel="stopHold"
          class="w-16 h-16 rounded-full bg-black flex items-center justify-center text-lime shadow-lg hover:scale-110 transition-transform"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="30"
            height="30"
            viewBox="0 0 24 24"
            fill="#8fe507"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path d="M7 10v12" />
            <path
              d="M15 5.88 14 10h5.83a2 2 0 0 1 1.92 2.56l-2.33 8A2 2 0 0 1 17.5 22H4a2 2 0 0 1-2-2v-8a2 2 0 0 1 2-2h2.76a2 2 0 0 0 1.79-1.11L12 2h0a3.13 3.13 0 0 1 3 3.88Z"
            />
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { SelfBuildingSquareSpinner } from "epic-spinners";
import HeaderComp from '../components/HeaderComp.vue'

export default {
  components: { 
    HeaderComp,
    SelfBuildingSquareSpinner 
  },
  name: "HomeView",
  data() {
    return {
      isLoading: true,
      currentIndex: 0,
      holdAnimId: null,
      holdDirection: null,
      touchStartX: 0,
      touchCurrentX: 0,
      isSwiping: false,
      releaseActive: false,
      releaseTargetX: 0,
      releaseRotate: 0,
      stages: [
        {
          id: 1,
          company: "Studio Dumbar",
          title: "Visual Design Stagiair",
          location: "Rotterdam",
          image:
            "https://images.unsplash.com/photo-1497215728101-856f4ea42174?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80",
          tags: ["Adobe CC", "Branding", "MBO4"],
        },
        {
          id: 2,
          company: "Coolblue",
          title: "UX/UI Designer",
          location: "Rotterdam",
          image:
            "https://images.unsplash.com/photo-1556761175-5973dc0f32e7?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80",
          tags: ["Figma", "Research", "Agile"],
        },
        {
          id: 3,
          company: "Dept Agency",
          title: "Frontend Developer",
          location: "Amsterdam",
          image:
            "https://images.unsplash.com/photo-1542744173-8e7e53415bb0?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80",
          tags: ["React", "CSS", "Creative"],
        },
      ],
    };
  },
  mounted() {
    // simulate network loading delay then show stages
    setTimeout(() => {
      this.isLoading = false;
    }, 900);
  },
  computed: {
    currentStage() {
      return this.stages[this.currentIndex];
    },
    displayStack() {
      // return up to 3 stages: current + next two
      const out = [];
      for (let i = 0; i < Math.min(3, this.stages.length); i++) {
        out.push(this.stages[(this.currentIndex + i) % this.stages.length]);
      }
      return out;
    },
    swipeDelta() {
      return this.isSwiping ? this.touchCurrentX - this.touchStartX : 0;
    },
    swipeOpacity() {
      return this.releaseActive
        ? 1
        : Math.min(Math.abs(this.swipeDelta) / 120, 1);
    },
    cardStyle() {
      if (this.releaseActive) {
        const dx = this.releaseTargetX;
        const rotate = this.releaseRotate;
        return {
          transform: `translateX(${dx}px) rotate(${rotate}deg)`,
          transition: "transform 300ms ease",
        };
      }
      const dx = this.swipeDelta;
      const rotate = dx / 20;
      return {
        transform: `translateX(${dx}px) rotate(${rotate}deg)`,
        transition: this.isSwiping ? "none" : "transform 300ms ease",
      };
    },
    // helper to style stacked cards (index 0 is top)
    stackCardStyle() {
      return (stackIndex) => {
        if (stackIndex === 0) return this.cardStyle;
        // base offset + scale for depth
        const translateY = stackIndex * 12;
        const scale = 1 - stackIndex * 0.04;
        // if releaseActive, bring the next card forward
        if (this.releaseActive && stackIndex === 1) {
          return {
            transform: "translateY(0px) scale(1)",
            transition: "transform 300ms ease",
            zIndex: 20 - stackIndex,
          };
        }
        return {
          transform: `translateY(${translateY}px) scale(${scale})`,
          transition: "transform 300ms ease",
          zIndex: 20 - stackIndex,
        };
      };
    },
  },
  methods: {
    nextStage() {
      this.currentIndex = (this.currentIndex + 1) % this.stages.length;
    },
    prevStage() {
      this.currentIndex =
        (this.currentIndex - 1 + this.stages.length) % this.stages.length;
    },

    startHold(direction) {
      if (this.isLoading || this.releaseActive) return;
      this.holdDirection = direction;
      this.isSwiping = true;
      this.touchStartX = (window.innerWidth || 800) / 2;
      this.touchCurrentX = this.touchStartX;
      const sign = direction === 'right' ? 1 : -1;
      const targetOffset = sign * Math.min(220, (window.innerWidth || 800) * 0.35);

      const step = () => {
        const target = this.touchStartX + targetOffset;
        this.touchCurrentX += (target - this.touchCurrentX) * 0.16;
        this.holdAnimId = requestAnimationFrame(step);
      };
      this.holdAnimId = requestAnimationFrame(step);
    },

    stopHold() {
      if (this.holdAnimId) {
        cancelAnimationFrame(this.holdAnimId);
        this.holdAnimId = null;
      }
      if (this.isSwiping) this.onTouchEnd();
      this.holdDirection = null;
    },

    onTouchStart(e) {
      const t = e.touches && e.touches[0];
      if (!t) return;
      this.touchStartX = t.clientX;
      this.touchCurrentX = t.clientX;
      this.isSwiping = true;
    },
    onTouchMove(e) {
      if (!this.isSwiping) return;
      const t = e.touches && e.touches[0];
      if (!t) return;
      this.touchCurrentX = t.clientX;
    },
    onTouchEnd() {
      if (!this.isSwiping) return;
      const delta = this.touchCurrentX - this.touchStartX;
      const threshold = 50;
      // decisive swipe -> animate offscreen then change stage
      if (delta <= -threshold) {
        // swipe left -> dislike
        this.releaseActive = true;
        this.releaseTargetX = -(window.innerWidth || 800);
        this.releaseRotate = -20;
        // let the animation play, then advance
        setTimeout(() => {
          this.nextStage();
          this.releaseActive = false;
          // reset touch state
          this.touchStartX = 0;
          this.touchCurrentX = 0;
        }, 300);
      } else if (delta >= threshold) {
        // swipe right -> like
        this.releaseActive = true;
        this.releaseTargetX = window.innerWidth || 800;
        this.releaseRotate = 20;
        setTimeout(() => {
          this.nextStage();
          this.releaseActive = false;
          this.touchStartX = 0;
          this.touchCurrentX = 0;
        }, 300);
      } else {
        // not decisive -> snap back
        this.isSwiping = false;
        this.touchStartX = 0;
        this.touchCurrentX = 0;
      }
      // stop swiping interaction; releaseActive controls animation when set
      if (!this.releaseActive) this.isSwiping = false;
    },
  },
};
</script>

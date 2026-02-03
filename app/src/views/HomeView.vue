<template>
  <div class="h-full w-full bg-[#F4F4F4] flex flex-col">
      <HeaderComp />

      <main id="main" class="flex-1 w-full mx-auto px-3 py-2 relative flex flex-col overflow-hidden">
        <div
          v-if="isLoading"
          id="loader"
          class="w-full h-full flex items-center justify-center flex-col"
        >
          <self-building-square-spinner
            :animation-duration="6000"
            :size="40"
            color="#8fe507"
          />
        </div>
        
        <div v-else-if="currentStage" id="card-stack" class="w-full flex-1 relative min-h-0">
          <div id="card-stack-inner" class="relative w-full h-full min-h-0 pb-14 md:pb-18 mx-auto max-w-[640px]">
            <template
              v-for="(stage, stackIndex) in displayStack"
              :key="stage.id"
            >
              <div
                v-if="stackIndex === 0"
                id="top-card"
                class="absolute inset-0 z-30 rounded-3xl overflow-hidden shadow-xl bg-white border border-gray-200 flex flex-col"
                :style="stackCardStyle(stackIndex)"
                @touchstart.passive="onTouchStart"
                @touchmove.passive="onTouchMove"
                @touchend.passive="onTouchEnd"
              >
                <div id="card-image" class="h-[70%] md:h-[55%] w-full relative bg-gray-300">
                  <img :src="stage.image" class="w-full h-full object-cover" />
                  <div
                    class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent"
                  ></div>
                </div>

                <div
                  id="card-content"
                  class="flex-none h-[20%] md:h-[30%] p-3 flex flex-col justify-start bg-white relative -mt-4 z-10 overflow-hidden"
                  style="min-height:90px"
                >
                  <h2 class="font-oswald font-bold text-xl md:text-2xl mb-1 text-black">
                    {{ stage.company }}
                  </h2>
                  <p
                    class="font-inter font-semibold text-gray-500 text-sm md:text-base mb-3"
                  >
                    {{ stage.title }}
                  </p>

                  <div id="tags" class="flex flex-wrap gap-2">
                    <span
                      v-for="(tag, index) in stage.tags"
                      :key="index"
                      class="px-3 py-0.5 bg-gray-200 rounded-full font-inter text-xs font-medium text-gray-800"
                      >{{ tag }}</span
                    >
                  </div>

                  <div id="location" class="mt-4 flex items-center text-gray-500 font-inter text-sm">
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

                <!-- Card action buttons (moved into card to save vertical space) -->
                <div id="card-actions" class="mt-4 flex items-center justify-between gap-3 px-4">
                  <button id="btn-dislike-card" class="flex-1 h-10 rounded-full bg-red-500 border border-transparent flex items-center justify-center text-white shadow-sm" @mousedown.prevent="startHold('left')" @mouseup="stopHold" @mouseleave="stopHold" @touchstart.prevent="startHold('left')" @touchend="stopHold" @touchcancel="stopHold" @click="nextStage">
                    <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M10 15v4a3 3 0 0 0 3 3l4-9V2H5.72a2 2 0 0 0-2 1.7l-1.38 9a2 2 0 0 0 2 2.3zm7-13h2.67A2.31 2.31 0 0 1 22 4v7a2.31 2.31 0 0 1-2.33 2H17"/></svg>
                  </button>

                  <button id="btn-center-card" class="w-10 h-10 rounded-full bg-black flex items-center justify-center text-white shadow-sm">
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="16" x2="12" y2="12"/><line x1="12" y1="8" x2="12.01" y2="8"/></svg>
                  </button>

                  <button id="btn-like-card" class="flex-1 h-10 rounded-full bg-green-500 flex items-center justify-center text-white shadow-sm" @mousedown.prevent="startHold('right')" @mouseup="stopHold" @mouseleave="stopHold" @touchstart.prevent="startHold('right')" @touchend="stopHold" @touchcancel="stopHold" @click="nextStage">
                    <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M14 9V5a3 3 0 0 0-3-3l-4 9v11h11.28a2 2 0 0 0 2-1.7l1.38-9a2 2 0 0 0-2-2.3zM7 22H4a2 2 0 0 1-2-2v-7a2 2 0 0 1 2-2h3"/></svg>
                  </button>
                </div>

                <!-- Top card overlay indicators -->
                <div class="absolute inset-0 pointer-events-none z-40">
                  <div
                    id="overlay-nope"
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
                    id="overlay-like"
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
                <div class="h-[50%] md:h-[60%] w-full relative bg-gray-300">
                  <img :src="stage.image" class="w-full h-full object-cover" />
                  <div
                    class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent"
                  ></div>
                </div>
                <div
                  class="flex-none h-[28%] md:h-[30%] p-3 flex flex-col justify-start bg-white relative -mt-4 rounded-t-3xl z-10 overflow-hidden"
                  style="min-height:90px"
                >
                  <h2 class="font-oswald font-bold text-2xl md:text-3xl mb-1 text-black">
                    {{ stage.company }}
                  </h2>
                  <p
                    class="font-inter font-semibold text-gray-500 text-base md:text-lg mb-4"
                  >
                    {{ stage.title }}
                  </p>

                  <div id="card-actions" class="mt-3 flex items-center justify-between gap-3 px-2">
                    <button id="btn-dislike-card-2" class="flex-1 h-9 rounded-full bg-red-500 border border-transparent flex items-center justify-center text-white shadow-sm" @mousedown.prevent="startHold('left')" @mouseup="stopHold" @mouseleave="stopHold" @touchstart.prevent="startHold('left')" @touchend="stopHold" @touchcancel="stopHold" @click="nextStage">
                      <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M10 15v4a3 3 0 0 0 3 3l4-9V2H5.72a2 2 0 0 0-2 1.7l-1.38 9a2 2 0 0 0 2 2.3zm7-13h2.67A2.31 2.31 0 0 1 22 4v7a2.31 2.31 0 0 1-2.33 2H17"/></svg>
                    </button>
                    <button id="btn-center-card-2" class="w-9 h-9 rounded-full bg-black flex items-center justify-center text-white shadow-sm">
                      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="16" x2="12" y2="12"/><line x1="12" y1="8" x2="12.01" y2="8"/></svg>
                    </button>
                    <button id="btn-like-card-2" class="flex-1 h-9 rounded-full bg-green-500 flex items-center justify-center text-white shadow-sm" @mousedown.prevent="startHold('right')" @mouseup="stopHold" @mouseleave="stopHold" @touchstart.prevent="startHold('right')" @touchend="stopHold" @touchcancel="stopHold" @click="nextStage">
                      <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M14 9V5a3 3 0 0 0-3-3l-4 9v11h11.28a2 2 0 0 0 2-1.7l1.38-9a2 2 0 0 0-2-2.3zM7 22H4a2 2 0 0 1-2-2v-7a2 2 0 0 1 2-2h3"/></svg>
                    </button>
                  </div>
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
      </main>

      

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
        // base offset + scale for depth (smaller offsets to shrink deck)
        const translateY = stackIndex * 8;
        const scale = 1 - stackIndex * 0.03;
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

<template>
  <div class="h-[calc(100%-80px)] w-full overflow-hidden">
    <div class="flex flex-col h-full bg-[#F4F4F4]">
      <div class="px-5 py-3 flex justify-between items-center bg-[#F4F4F4]">
        <div class="flex items-center gap-3">
          <img src="/logo-stagematch.png" alt="StageMatch" class="h-10 w-auto object-contain" />
          <span class="font-oswald font-bold text-lg tracking-wide">StageMatch</span>
        </div>
        <button aria-label="Settings" class="w-10 h-10 bg-white border rounded-full flex items-center justify-center text-gray-700 hover:bg-gray-100 transition">
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0 .17.7.66 1.14 1.37 1.18 1.693.12 3.16 1.59 3.28 3.28.04.71.48 1.2 1.18 1.37 1.756.426 1.756 2.924 0 3.35-.7.17-1.14.66-1.18 1.37-.12 1.693-1.59 3.16-3.28 3.28-.71.04-1.2.48-1.37 1.18-.426 1.756-2.924 1.756-3.35 0-.17-.7-.66-1.14-1.37-1.18-1.693-.12-3.16-1.59-3.28-3.28-.04-.71-.48-1.2-1.18-1.37-1.756-.426-1.756-2.924 0-3.35.7-.17 1.14-.66 1.18-1.37.12-1.693 1.59-3.16 3.28-3.28.71-.04 1.2-.48 1.37-1.18z" />
            <path d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
          </svg>
        </button>
      </div>

      <div class="flex-1 px-4 py-2 relative">
        <div v-if="currentStage" class="w-full h-full bg-white rounded-3xl overflow-hidden shadow-xl relative border border-gray-200 flex flex-col">
          <div class="h-[65%] w-full relative bg-gray-300">
            <img :src="currentStage.image" class="w-full h-full object-cover" />
            <div class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent"></div>
          </div>

          <div class="flex-1 p-5 flex flex-col justify-center bg-white relative -mt-4 rounded-t-3xl z-10">
            <h2 class="font-oswald font-bold text-3xl mb-1 text-black">{{ currentStage.company }}</h2>
            <p class="font-inter font-semibold text-gray-500 text-lg mb-4">{{ currentStage.title }}</p>

            <div class="flex flex-wrap gap-2">
              <span v-for="(tag, index) in currentStage.tags" :key="index" class="px-4 py-1 bg-gray-200 rounded-full font-inter text-sm font-medium text-gray-800">{{ tag }}</span>
            </div>

            <div class="mt-4 flex items-center text-gray-500 font-inter text-sm">
              <svg class="mr-1" xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z"/><circle cx="12" cy="10" r="3"/></svg>
              {{ currentStage.location }}
            </div>
          </div>
        </div>
        <div v-else class="flex h-full items-center justify-center text-center p-8">
          <div>
            <h3 class="font-oswald text-2xl mb-2">Helemaal bij!</h3>
            <p class="text-gray-500">Er zijn geen nieuwe stages meer.</p>
          </div>
        </div>
      </div>

      <div class="h-24 px-8 flex items-center justify-between pb-4">
        <button @click="nextStage" class="w-16 h-16 rounded-full bg-white border-2 border-gray-200 flex items-center justify-center text-black shadow-lg hover:scale-110 transition-transform">
          <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17 14V2"/><path d="M9 18.12 10 14H4.17a2 2 0 0 1-1.92-2.56l2.33-8A2 2 0 0 1 6.5 2H19a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2h-2.76a2 2 0 0 0-1.79 1.11L12 22h0a3.13 3.13 0 0 1-3-3.88Z"/></svg>
        </button>

        <button class="w-12 h-12 rounded-full bg-black flex items-center justify-center text-white shadow-lg hover:scale-110 transition-transform">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M12 16v-4"/><path d="M12 8h.01"/></svg>
        </button>

        <button @click="nextStage" class="w-16 h-16 rounded-full bg-black flex items-center justify-center text-lime shadow-lg hover:scale-110 transition-transform">
          <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 24 24" fill="#8fe507" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M7 10v12"/><path d="M15 5.88 14 10h5.83a2 2 0 0 1 1.92 2.56l-2.33 8A2 2 0 0 1 17.5 22H4a2 2 0 0 1-2-2v-8a2 2 0 0 1 2-2h2.76a2 2 0 0 0 1.79-1.11L12 2h0a3.13 3.13 0 0 1 3 3.88Z"/></svg>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
  data() {
    return {
      currentIndex: 0,
      stages: [
        { id: 1, company: 'Studio Dumbar', title: 'Visual Design Stagiair', location: 'Rotterdam', image: 'https://images.unsplash.com/photo-1497215728101-856f4ea42174?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80', tags: ['Adobe CC','Branding','MBO4'] },
        { id: 2, company: 'Coolblue', title: 'UX/UI Designer', location: 'Rotterdam', image: 'https://images.unsplash.com/photo-1556761175-5973dc0f32e7?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80', tags: ['Figma','Research','Agile'] },
        { id: 3, company: 'Dept Agency', title: 'Frontend Developer', location: 'Amsterdam', image: 'https://images.unsplash.com/photo-1542744173-8e7e53415bb0?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80', tags: ['React','CSS','Creative'] }
      ]
    }
  },
  computed: {
    currentStage() { return this.stages[this.currentIndex] }
  },
  methods: {
    nextStage() { this.currentIndex = (this.currentIndex + 1) % this.stages.length }
  }
}
</script>

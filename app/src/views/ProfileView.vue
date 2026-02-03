<template>
  <div class="flex flex-col h-full bg-[#F4F4F4] overflow-y-auto pb-4">
    <div class="px-5 py-4 bg-[#F4F4F4] flex justify-between items-center sticky top-0 z-10 border-b border-gray-200">
      <h1 class="font-oswald font-bold text-2xl uppercase tracking-wide text-black">MIJN PROFIEL</h1>
      <div class="text-black transform rotate-90">
        <svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m21 21-6-6m6 6v-4.8m0 4.8h-4.8"/><path d="M3 16.2V21m0 0h4.8M3 21l6-6"/><path d="M21 7.8V3m0 0h-4.8M21 3l-6 6"/><path d="M3 7.8V3m0 0h4.8M3 3l6 6"/></svg>
      </div>
    </div>

    <div class="px-5 flex flex-col items-center mt-4">
      <div class="w-32 h-32 bg-gray-300 rounded-full mb-4 relative overflow-hidden shadow-inner">
        <img src="https://images.unsplash.com/photo-1599566150163-29194dcaad36?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80" class="w-full h-full object-cover" />
      </div>

      <h2 class="font-oswald font-bold text-2xl mb-1">Jan Jansen</h2>
      <p class="font-oswald font-medium text-lg text-center leading-tight mb-2">Creative Software Developer<br/>MBO4</p>
      <div class="flex items-center text-gray-600 font-bold mb-6">Rotterdam</div>

      <div class="w-48 mb-8">
        <button class="w-full py-3 bg-[#e0e0e0] font-oswald font-bold text-black rounded text-lg border border-gray-300 shadow-sm">Bewerken</button>
      </div>

      <div class="w-full mb-8">
        <div class="flex flex-wrap justify-center gap-2">
          <span class="inline-flex items-center px-4 py-1.5 rounded-full text-sm font-medium mr-2 mb-2 bg-[#e0e0e0] text-[#1A1A1A]">HTML/CSS</span>
          <span class="inline-flex items-center px-4 py-1.5 rounded-full text-sm font-medium mr-2 mb-2 bg-[#e0e0e0] text-[#1A1A1A]">React</span>
          <span class="inline-flex items-center px-4 py-1.5 rounded-full text-sm font-medium mr-2 mb-2 bg-[#e0e0e0] text-[#1A1A1A]">Design</span>
          <span class="inline-flex items-center px-4 py-1.5 rounded-full text-sm font-medium mr-2 mb-2 bg-[#dcdcdc] text-black cursor-pointer">+</span>
        </div>
      </div>

      <div class="w-3/4 h-px bg-black mb-6"></div>

      <div class="w-full pb-10">
        <h3 class="font-oswald font-bold text-xl mb-4 uppercase">Ervaring / Opleiding</h3>
        <div class="space-y-4">
          <div class="flex gap-4 items-start">
            <div class="mt-1">
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m4 6 8-4 8 4"/><path d="m18 10 4 2v8a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2v-8l4-2"/><path d="M14 22v-4a2 2 0 0 0-2-2v0a2 2 0 0 0-2 2v4"/><path d="M18 5v17"/><path d="M6 5v17"/><circle cx="12" cy="9" r="2"/></svg>
            </div>
            <div>
              <div class="font-oswald font-bold">Grafisch Lyceum Rotterdam</div>
              <div class="font-inter text-sm text-gray-600">Software Developer • 2022 - Heden</div>
            </div>
          </div>
          <div class="flex gap-4 items-start">
            <div class="mt-1">
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="20" height="14" x="2" y="7" rx="2" ry="2"/><path d="M16 21V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v16"/></svg>
            </div>
            <div>
              <div class="font-oswald font-bold">Albert Heijn</div>
              <div class="font-inter text-sm text-gray-600">Vakkenvuller • 2020 - 2022</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProfileView'
}
</script>

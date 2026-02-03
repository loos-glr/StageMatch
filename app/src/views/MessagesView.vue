<template>
  <div class="flex flex-col h-full bg-[#F4F4F4]">
    <header-comp title="BERICHTEN" />
    <div class="flex-1 overflow-y-auto">
      <div class="px-5 py-4">
        <h3 class="font-oswald font-bold text-lg mb-3 uppercase">Matches</h3>
        <div class="flex gap-4 overflow-x-auto pb-2 scrollbar-hide">
          <div v-for="match in matches" :key="match.id" class="flex-shrink-0 relative cursor-pointer">
            <img :src="match.img" class="w-16 h-16 rounded-full border-2 border-white shadow-sm object-cover" />
            <div class="absolute top-0 right-0 w-4 h-4 bg-lime rounded-full border-2 border-white"></div>
          </div>
        </div>
      </div>

      <div class="px-5 pb-4">
        <h3 class="font-oswald font-bold text-lg mb-3 uppercase">Chats</h3>
        <div class="flex flex-col gap-4">
          <div v-for="chat in chats" :key="chat.id" class="flex items-center p-3 bg-white rounded-lg shadow-sm active:bg-gray-50 transition-colors cursor-pointer">
            <div class="w-14 h-14 rounded-full bg-black text-white flex items-center justify-center font-bold mr-4 shrink-0 overflow-hidden">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>
            </div>
            <div class="flex-1 min-w-0">
              <div class="flex justify-between items-baseline mb-1">
                <h4 class="font-oswald font-bold text-base truncate pr-2">{{ chat.name }}</h4>
                <span class="text-xs text-gray-400 font-inter whitespace-nowrap">{{ chat.time }}</span>
              </div>
              <p class="font-inter text-sm text-gray-500 truncate">{{ chat.msg }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import HeaderComp from '../components/HeaderComp.vue'

export default {
  name: 'MessagesView',
  components: { HeaderComp },
  data() {
    return {
      matches: [
        { id: 1, img: 'https://ui-avatars.com/api/?name=Coolblue&background=2D7DD2&color=fff' },
        { id: 2, img: 'https://ui-avatars.com/api/?name=DPG+Media&background=eb3b5a&color=fff' },
        { id: 3, img: 'https://ui-avatars.com/api/?name=MediaMonks&background=000&color=fff' },
        { id: 4, img: 'https://ui-avatars.com/api/?name=KPN&background=009900&color=fff' }
      ],
      chats: [
        { id: 1, name: 'Grafisch Lyceum Rotterdam', msg: 'Hallo Jan, we zouden je graag op...', time: '09:41' },
        { id: 2, name: 'Studio Dumbar', msg: 'Heb je een portfolio link?', time: 'Gisteren' },
        { id: 3, name: 'Agency X', msg: 'Bedankt voor je interesse!', time: 'Dinsdag' },
        { id: 4, name: 'Rotterdam Design', msg: 'Wanneer kan je starten?', time: 'Maandag' },
        { id: 5, name: 'TechHub', msg: 'Ziet er goed uit!', time: 'Zondag' }
      ]
    }
  }
}
</script>

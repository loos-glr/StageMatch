<template>
  <div class="h-screen w-full bg-white text-[#1A1A1A] flex flex-col overflow-hidden">
    <div class="flex-1 relative w-full overflow-hidden">
      <component :is="currentView" v-bind:active-tab="activeTab" @update:active-tab="val => activeTab = val" class="h-full w-full" />
    </div>

    <nav class="flex-none w-full h-20 bg-white border-t border-gray-200 flex justify-around items-center px-2 z-20">
      <button @click="activeTab = 'home'" :class="activeTab === 'home' ? 'text-lime' : 'text-gray-400 hover:text-gray-600'" class="flex flex-col items-center justify-center w-16 h-16 rounded-full transition-all">
        <svg xmlns="http://www.w3.org/2000/svg" :width="activeTab === 'home' ? 32 : 28" :height="activeTab === 'home' ? 32 : 28" viewBox="0 0 24 24" :fill="activeTab === 'home' ? 'currentColor' : 'none'" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M8.5 14.5A2.5 2.5 0 0 0 11 12c0-1.38-.5-2-1-3-1.072-2.143-.224-4.054 2-6 .5 2.5 2 4.9 4 6.5 2 1.6 3 3.5 3 5.5a7 7 0 1 1-14 0c0-1.153.433-2.294 1-3a2.5 2.5 0 0 0 2.5 2.5z"/></svg>
      </button>

      <button @click="activeTab = 'messages'" :class="activeTab === 'messages' ? 'text-lime' : 'text-gray-400 hover:text-gray-600'" class="flex flex-col items-center justify-center w-16 h-16 rounded-full transition-all">
        <svg xmlns="http://www.w3.org/2000/svg" :width="activeTab === 'messages' ? 32 : 28" :height="activeTab === 'messages' ? 32 : 28" viewBox="0 0 24 24" :fill="activeTab === 'messages' ? 'currentColor' : 'none'" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M7.9 20A9 9 0 1 0 4 16.1L2 22Z"/></svg>
      </button>

      <button @click="activeTab = 'profile'" :class="activeTab === 'profile' ? 'text-lime' : 'text-gray-400 hover:text-gray-600'" class="flex flex-col items-center justify-center w-16 h-16 rounded-full transition-all">
        <svg xmlns="http://www.w3.org/2000/svg" :width="activeTab === 'profile' ? 32 : 28" :height="activeTab === 'profile' ? 32 : 28" viewBox="0 0 24 24" :fill="activeTab === 'profile' ? 'currentColor' : 'none'" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>
      </button>
    </nav>
  </div>
</template>

<script>
import HomeView from './views/HomeView.vue'
import MessagesView from './views/MessagesView.vue'
import ProfileView from './views/ProfileView.vue'

export default {
  name: 'App',
  components: { HomeView, MessagesView, ProfileView },
  data() {
    return { activeTab: 'home' }
  },
  computed: {
    currentView() {
      return this.activeTab === 'home' ? 'HomeView' : (this.activeTab === 'messages' ? 'MessagesView' : 'ProfileView')
    }
  }
}
</script>

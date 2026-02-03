<template>
  <header class="px-5 py-4 bg-[#F4F4F4] flex justify-between items-center sticky top-0 z-10 border-b border-gray-200">
    <div class="flex items-center gap-3">
      <img src="/logo-stagematch.png" alt="StageMatch" class="h-10 w-auto object-contain" />
      <span class="font-oswald font-bold text-lg tracking-wide text-black">StageMatch</span>
    </div>

    <div class="flex-1 text-center">
      <h1 v-if="title" class="font-oswald font-bold text-2xl uppercase tracking-wide text-black">{{ title }}</h1>
    </div>

    <div class="w-8" />
  </header>
</template>

<script>
export default {
  name: 'HeaderComp',
  props: { title: { type: String, default: '' } }
}
</script>

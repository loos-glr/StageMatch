<template>
  <header class="px-5 py-4 bg-[#F4F4F4] flex justify-between items-center sticky top-0 z-10 border-b border-gray-200">
    <h1 class="font-oswald font-bold text-2xl uppercase tracking-wide text-black">{{ title }}</h1>
  </header>
</template>

<script>
export default {
  name: 'HeaderComp',
  props: { title: { type: String, default: '' } }
}
</script>

<template>
  <div class="px-5 py-3 flex justify-between items-center bg-[#F4F4F4]">
        <div class="flex items-center gap-3">
          <img
            src="/logo-stagematch.png"
            alt="StageMatch"
            class="h-10 w-auto object-contain"
          />
          <span class="font-oswald font-bold text-lg tracking-wide"
            >StageMatch</span
          >
        </div>
        <button
          aria-label="Settings"
          class="w-10 h-10 bg-white border rounded-full flex items-center justify-center text-gray-700 hover:bg-gray-100 transition"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="20"
            height="20"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.5"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path
              d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0 .17.7.66 1.14 1.37 1.18 1.693.12 3.16 1.59 3.28 3.28.04.71.48 1.2 1.18 1.37 1.756.426 1.756 2.924 0 3.35-.7.17-1.14.66-1.18 1.37-.12 1.693-1.59 3.16-3.28 3.28-.71.04-1.2.48-1.37 1.18-.426 1.756-2.924 1.756-3.35 0-.17-.7-.66-1.14-1.37-1.18-1.693-.12-3.16-1.59-3.28-3.28-.04-.71-.48-1.2-1.18-1.37-1.756-.426-1.756-2.924 0-3.35.7-.17 1.14-.66 1.18-1.37.12-1.693 1.59-3.16 3.28-3.28.71-.04 1.2-.48 1.37-1.18z"
            />
            <path d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
          </svg>
        </button>
      </div>
</template>

<script>
export default {
  name: 'HeaderComp',
  props: { title: { type: String, default: '' } }
}
</script>

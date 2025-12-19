<script setup>
defineProps({
  app: {
    type: Object,
    required: true
  }
});
</script>

<template>
  <div
    class="p-5 rounded-xl border border-white/10 bg-white/5 backdrop-blur-lg shadow-lg hover:-translate-y-1 hover:shadow-2xl hover:border-[#1767FF]/50 hover:bg-white/10 transition-all duration-300 cursor-pointer">
    <div class="flex items-center gap-3">
      <img 
        v-if="app.f_icon_url" 
        :src="app.f_icon_url.replace('i.navy.mi.th', 'i.localhost')" 
        :alt="app.f_app_title"
        class="w-12 h-12 rounded-lg object-cover flex-shrink-0"
      />
      <div v-else class="w-12 h-12 rounded-lg bg-[#1767FF]/30 flex items-center justify-center text-2xl flex-shrink-0">
        📱
      </div>
      <div class="text-lg font-semibold">{{ app.f_app_title }}</div>
    </div>
  </div>
</template>

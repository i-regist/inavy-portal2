<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const banners = [
  '/banner-l-1.png',
  '/banner-l-2.jfif',
  '/banner-l-3.png',
  '/banner-l-4.png',
  '/banner-l-5.png',
  '/banner-l-6.png',
  '/banner-l-8.jpg',
  '/banner-l-9.png',
  '/banner-l-10.png'
];

const currentBanner = ref(0);
let autoScrollInterval = null;

const nextBanner = () => {
  currentBanner.value = (currentBanner.value + 1) % banners.length;
};

const prevBanner = () => {
  currentBanner.value = (currentBanner.value - 1 + banners.length) % banners.length;
};

const startAutoScroll = () => {
  autoScrollInterval = setInterval(() => {
    nextBanner();
  }, 5000); // Change banner every 5 seconds
};

const stopAutoScroll = () => {
  if (autoScrollInterval) {
    clearInterval(autoScrollInterval);
    autoScrollInterval = null;
  }
};

onMounted(() => {
  startAutoScroll();
});

onUnmounted(() => {
  stopAutoScroll();
});
</script>

<template>
  <div class="relative rounded-2xl border border-white/10 bg-white/5 backdrop-blur-lg overflow-hidden shadow-lg">
    <div class="relative h-48">
      <img 
        :src="banners[currentBanner]" 
        :alt="`Banner ${currentBanner + 1}`"
        class="w-full h-full object-cover"
      />
      
      <!-- Previous Button -->
      <button 
        @click="prevBanner"
        class="absolute left-4 top-1/2 -translate-y-1/2 p-2 rounded-full bg-white/20 hover:bg-white/30 backdrop-blur-md transition-all">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
        </svg>
      </button>
      
      <!-- Next Button -->
      <button 
        @click="nextBanner"
        class="absolute right-4 top-1/2 -translate-y-1/2 p-2 rounded-full bg-white/20 hover:bg-white/30 backdrop-blur-md transition-all">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
        </svg>
      </button>
      
      <!-- Indicators -->
      <div class="absolute bottom-4 left-1/2 -translate-x-1/2 flex gap-2">
        <button
          v-for="(banner, index) in banners"
          :key="index"
          @click="currentBanner = index"
          class="w-2 h-2 rounded-full transition-all"
          :class="currentBanner === index ? 'bg-white w-8' : 'bg-white/50'"
        ></button>
      </div>
    </div>
  </div>
</template>

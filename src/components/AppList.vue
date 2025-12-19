<script setup>
import { ref, onMounted } from 'vue';
import AppCard from './AppCard.vue';

// Fetch app data from API
const appData = ref(null);
const loading = ref(false);
const error = ref(null);

const fetchAppData = async () => {
  loading.value = true;
  error.value = null;
  
  try {
    const response = await fetch('http://iservice.localhost/?r=api/app');
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json();
    appData.value = data;
    console.log('App data fetched:', data);
  } catch (err) {
    error.value = err.message;
    console.error('Error fetching app data:', err);
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchAppData();
});
</script>

<template>
  <div>
    <!-- Loading State -->
    <div v-if="loading" class="text-center py-8 text-[#B8C6D9]">
      Loading apps...
    </div>
    
    <!-- Error State -->
    <div v-else-if="error" class="p-5 rounded-xl border border-red-500/30 bg-red-500/10 text-red-300">
      Error: {{ error }}
    </div>
    
    <!-- App Cards -->
    <div v-else-if="appData && appData.length > 0">
      <div class="grid grid-cols-1 gap-2">
        <AppCard 
          v-for="app in appData" 
          :key="app.f_app_id"
          :app="app"
        />
      </div>
    </div>
  </div>
</template>

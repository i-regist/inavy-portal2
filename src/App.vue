<script setup>
import { ref, onMounted, computed } from "vue";
import AppList from "./components/AppList.vue";
import BannerCarousel from "./components/BannerCarousel.vue";

const navItems = ["Dashboard", "Tasks", "Approvals", "Reports", "Settings"];

const notifications = ref([]);
const showNotifications = ref(false);
const showSidebar = ref(true);

const unreadCount = computed(() => {
  return notifications.value.filter((n) => !n.is_read).length;
});

const fetchNotifications = async () => {
  try {
    const response = await fetch(
      "https://i.localhost/portal2/notification/load"
    );
    const data = await response.json();
    const rawNotifications = data.data || data; // Handle both {data: []} and []

    notifications.value = rawNotifications.map((n) => {
      let parsedData = n.data;
      if (typeof parsedData === "string") {
        try {
          parsedData = JSON.parse(parsedData);
          // Handle double-encoded JSON strings if necessary
          if (typeof parsedData === "string") {
            parsedData = JSON.parse(parsedData);
          }
        } catch (e) {
          console.error("Error parsing notification data", e);
        }
      }

      let message = n.message || "";
      if (parsedData && parsedData.person_name) {
        message = message.replace(/{name}/g, parsedData.person_name);
      }

      return {
        ...n,
        data: parsedData,
        message: message,
      };
    });
    console.log("Notifications fetched", notifications.value);
  } catch (error) {
    console.error("Failed to fetch notifications", error);
  }
};

const markAsRead = async (notification) => {
  if (notification.is_read) return;

  // Optimistic update
  notification.is_read = 1;

  try {
    await fetch(
      `https://i.localhost/portal2/notification/read?id=${notification.id}`
    );
  } catch (error) {
    console.error("Failed to mark notification as read", error);
  }
};

onMounted(() => {
  fetchNotifications();
});

const kpis = [
  { label: "Open Tasks", value: "1,234", icon: "📋" },
  { label: "Pending Approvals", value: "56", icon: "✅" },
  { label: "Reports Generated", value: "14", icon: "📊" },
];

const projects = [
  {
    id: 1,
    name: "Internal System Revamp",
    status: "In Progress",
    owner: "Nattapong",
  },
  { id: 2, name: "HR Portal Migration", status: "Completed", owner: "Siriwan" },
  { id: 3, name: "Network Upgrade", status: "Pending", owner: "Preecha" },
];
</script>

<template>
  <div
    class="min-h-screen bg-gradient-to-b from-[#071427] via-[#0B2447] to-[#051022] text-[#E6EFF9] flex flex-col">
    <!-- Header -->
    <header
      class="backdrop-blur-md bg-white/5 border-b border-white/10 px-6 py-3 flex justify-between items-center sticky top-0 z-10">
      <div class="text-lg font-semibold tracking-wide flex items-center gap-3">
        <button
          @click="showSidebar = !showSidebar"
          class="p-2 rounded-lg hover:bg-white/10 transition-colors mr-2 cursor-pointer">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M4 6h16M4 12h16M4 18h16" />
          </svg>
        </button>
        <div
          class="h-12 w-28 rounded-lg bg-white/100 p-1 flex items-center justify-center">
          <img
            src="/images/logo.png"
            alt="Logo"
            class="h-full w-full object-contain" />
        </div>
        ระบบงานภายในกองทัพเรือ
      </div>
      <div class="flex items-center gap-4">
        <input
          type="text"
          placeholder="Search..."
          class="px-3 py-1.5 bg-white/10 backdrop-blur-md rounded-lg text-sm focus:outline-none focus:ring-2 focus:ring-[#1767FF]/50" />
        
        <!-- Notification Button -->
        <button
          @click="showNotifications = !showNotifications"
          class="relative p-2 rounded-lg bg-white/10 hover:bg-white/20 transition-all">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-5 w-5"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
          </svg>
          <!-- Notification Badge -->
          <span
            v-if="unreadCount > 0"
            class="absolute -top-1 -right-1 h-4 w-4 bg-red-500 rounded-full text-xs flex items-center justify-center"
            >{{ unreadCount }}</span
          >

          <!-- Notification Dropdown -->
          <div
            v-if="showNotifications"
            class="absolute right-0 top-full mt-2 w-80 bg-[#0B2447] border border-white/10 rounded-lg shadow-xl z-50 overflow-hidden">
            <div class="p-3 border-b border-white/10 font-semibold text-left">
              การแจ้งเตือน
            </div>
            <div class="max-h-96 overflow-y-auto">
              <div
                v-for="notification in notifications"
                :key="notification.id"
                @click="markAsRead(notification)"
                class="p-3 border-b border-white/5 last:border-0 text-left cursor-pointer transition-colors"
                :class="[
                  notification.is_read
                    ? 'hover:bg-white/5 opacity-70'
                    : 'bg-white/10 hover:bg-white/15',
                ]">
                <div class="font-medium text-sm flex justify-between items-start">
                  {{ notification.title }}
                  <span
                    v-if="!notification.is_read"
                    class="h-2 w-2 bg-blue-500 rounded-full mt-1"></span>
                </div>
                <div
                  class="text-xs text-gray-400 mt-1"
                  v-html="notification.message"></div>
              </div>
              <div
                v-if="notifications.length === 0"
                class="p-4 text-center text-gray-400 text-sm">
                ไม่มีการแจ้งเตือน
              </div>
            </div>
          </div>
        </button>
        
        <div
          class="w-48 h-8 rounded-full bg-[#1767FF]/30 flex items-center justify-center text-sm">
          นายNavyoa1 navy0a1
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <main class="flex-1 flex gap-6 p-6">
      <!-- Sidebar -->
      <div v-show="showSidebar" class="transition-all duration-300">
        <AppList />
      </div>

      <!-- Dashboard Content -->
      <section class="flex-1 space-y-6">
        <!-- Banner Carousel -->
        <BannerCarousel />

        <!-- Calendar Widget -->
        <div class="grid grid-cols-2 gap-6 items-start">
          <div class="space-y-6">
            <div
              class="rounded-2xl border border-white/10 bg-white/5 backdrop-blur-lg overflow-hidden shadow-lg w-fit">
              <h2 class="text-xl font-semibold mb-3 text-center">ปฏิทิน</h2>
              <img
                src="/widget-calendar.jpg"
                alt="Calendar Widget"
                class="block" />
            </div>
          </div>

          <!-- Department Birthday Widget -->
          <div class="flex justify-end">
            <div class="space-y-6">
              <div
                class="rounded-2xl border border-white/10 bg-white/5 backdrop-blur-lg overflow-hidden shadow-lg w-fit">
                <h2 class="text-xl font-semibold mb-3 text-center">
                  วันสถาปนาหน่วย
                </h2>
                <div class="bg-white">
                  <img
                    src="/widget-dept-birth.jpg"
                    alt="Department Birthday Widget"
                    class="block" />
                </div>
              </div>

              <!-- Department Widget -->
              <div
                class="rounded-2xl border border-white/10 bg-white/5 backdrop-blur-lg overflow-hidden shadow-lg w-fit">
                <h2 class="text-xl font-semibold mb-3 text-center">หน่วยงาน</h2>
                <img
                  src="/widget-department.jpg"
                  alt="Department Widget"
                  class="block" />
              </div>
            </div>
          </div>
        </div>

        <!-- News Widget -->
        <div>
          <div
            class="rounded-2xl border border-white/10 bg-white/5 backdrop-blur-lg overflow-hidden shadow-lg w-fit">
            <h2 class="text-xl font-semibold mb-3 text-center">ข่าวสาร</h2>
            <img src="/widget-news.jpg" alt="News Widget" class="block" />
          </div>
        </div>
      </section>
    </main>

    <div
      id="problem-contact"
      class="p-5 rounded-xl border border-white/10 bg-white/5 backdrop-blur-lg shadow-lg hover:-translate-y-1 transition-transform text-center mx-6 mb-0">
      มีปัญหาการใช้งานกรุณาติดต่อที่<br />
      กองสนับสนุนไซเบอร์ ศซบ.สสท.ทร. 57807, 57809 (ในเวลาราชการ)
      และเวรศูนย์ไซเบอร์ฯ 57953, 0611290317 (นอกเวลาราชการ)<br />
      กรมการสื่อสารและเทคโนโลยีสารสนเทศทหารเรือ<br />
      พระราชวังเดิม ถ.วังเดิม แขวงวัดอรุณ เขตบางกอกใหญ่ กรุงเทพฯ 10600<br />
    </div>
    <!-- Footer -->
    <footer
      class="border-t border-white/10 text-[#B8C6D9] text-sm text-center py-3">
      © i.navy.mi.th #bkk-01 · v1.0
    </footer>
  </div>
</template>

<style scoped>
html,
body {
  font-family: "Inter", sans-serif;
}
</style>

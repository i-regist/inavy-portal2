<script setup>
const navItems = ["Dashboard", "Tasks", "Approvals", "Reports", "Settings"];

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
      <div class="text-lg font-semibold tracking-wide flex items-center gap-2">
        ระบบงานภายในกองทัพเรือ
      </div>
      <div class="flex items-center gap-4">
        <input
          type="text"
          placeholder="Search..."
          class="px-3 py-1.5 bg-white/10 backdrop-blur-md rounded-lg text-sm focus:outline-none focus:ring-2 focus:ring-[#1767FF]/50" />
        <div
          class="w-48 h-8 rounded-full bg-[#1767FF]/30 flex items-center justify-center text-sm">
          นายNavyoa1 navy0a1
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <main class="flex-1 flex gap-6 p-6">
      <!-- Sidebar -->
      <aside
        class="w-60 backdrop-blur-md bg-white/5 border border-white/10 rounded-2xl p-4 flex flex-col gap-3">
        <button
          v-for="item in navItems"
          :key="item"
          class="text-left px-3 py-2 rounded-lg hover:bg-[#1767FF]/20 transition-all">
          {{ item }}
        </button>
      </aside>

      <!-- Dashboard Content -->
      <section class="flex-1 space-y-6">
        <div>
          <h1 class="text-2xl font-semibold mb-1">Dashboard Overview</h1>
          <p class="text-[#B8C6D9] text-sm">
            Welcome back, here’s what’s happening today
          </p>
        </div>

        <!-- KPI Cards -->
        <div class="grid grid-cols-3 gap-6">
          <div
            v-for="kpi in kpis"
            :key="kpi.label"
            class="p-5 rounded-xl border border-white/10 bg-white/5 backdrop-blur-lg shadow-lg hover:-translate-y-1 transition-transform">
            <div class="text-3xl mb-2">{{ kpi.icon }}</div>
            <div class="text-xl font-semibold">{{ kpi.value }}</div>
            <div class="text-sm text-[#B8C6D9]">{{ kpi.label }}</div>
          </div>
        </div>

        <!-- Table Section -->
        <div
          class="rounded-2xl border border-white/10 bg-white/5 backdrop-blur-lg overflow-hidden">
          <table class="w-full text-left text-sm">
            <thead class="bg-white/10">
              <tr>
                <th class="py-3 px-4">#</th>
                <th class="py-3 px-4">Project</th>
                <th class="py-3 px-4">Status</th>
                <th class="py-3 px-4">Owner</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="row in projects"
                :key="row.id"
                class="hover:bg-white/10">
                <td class="py-3 px-4">{{ row.id }}</td>
                <td class="py-3 px-4">{{ row.name }}</td>
                <td class="py-3 px-4">{{ row.status }}</td>
                <td class="py-3 px-4">{{ row.owner }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>
    </main>

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

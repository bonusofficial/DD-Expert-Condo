<template>
  <div class="min-h-screen bg-slate-50 flex font-sans">
    <!-- Sidebar -->
    <AdminSidebar 
      :collapsed="isSidebarCollapsed" 
      :mobile-open="isMobileMenuOpen"
      @toggle-collapse="toggleSidebar"
      @close-mobile="closeMobileMenu"
    />

    <!-- Main Content -->
    <div 
      class="flex-1 flex flex-col min-h-screen transition-all duration-300 ease-in-out"
      :class="[isSidebarCollapsed ? 'lg:ml-20' : 'lg:ml-[280px]']"
    >
      <AdminHeader 
        @toggle-mobile="toggleMobileMenu"
        @toggle-collapse="toggleSidebar"
      />

      <main class="flex-1 p-4 lg:p-8 max-w-[1600px] w-full mx-auto animate-fade-in">
        <!-- Welcome Banner -->
        <div class="relative overflow-hidden rounded-3xl bg-gradient-to-br from-[#1a1a2e] via-[#2d2d54] to-[#1a1a2e] p-8 lg:p-10 mb-8 shadow-xl">
          <!-- Background Effects -->
          <div class="absolute inset-0 pointer-events-none">
            <div class="absolute top-0 left-0 w-full h-full bg-[radial-gradient(circle_at_20%_50%,rgba(218,165,32,0.15)_0%,transparent_40%)]"></div>
            <div class="absolute top-0 left-0 w-full h-full bg-[radial-gradient(circle_at_80%_30%,rgba(99,102,241,0.1)_0%,transparent_40%)]"></div>
          </div>

          <div class="relative z-10">
            <div class="max-w-2xl">
              <span class="inline-block text-[#DAA520] font-medium mb-2">สวัสดี {{ greeting }} 👋</span>
              <h1 class="text-3xl lg:text-4xl font-bold text-white mb-3 tracking-tight">ยินดีต้อนรับสู่ DDexpert Admin</h1>
              <p class="text-slate-300 text-lg">จัดการทรัพย์สินและเนื้อหาของคุณได้จากที่นี่</p>
            </div>
          </div>
        </div>

        <!-- Stats Grid -->
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
          <div v-for="stat in stats" :key="stat.id" class="bg-white rounded-2xl p-6 shadow-sm border border-slate-100 hover:-translate-y-1 transition-transform duration-300">
            <div class="flex justify-between items-start mb-4">
              <div 
                class="w-12 h-12 rounded-xl flex items-center justify-center"
                :class="{
                  'bg-amber-500/10 text-[#DAA520]': stat.color === 'gold',
                  'bg-orange-500/10 text-orange-500': stat.color === 'orange',
                  'bg-indigo-500/10 text-indigo-500': stat.color === 'purple',
                  'bg-emerald-500/10 text-emerald-500': stat.color === 'green',
                }"
              >
                <svg v-if="stat.id === 1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-6 h-6"><path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/></svg>
                <svg v-else-if="stat.id === 2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-6 h-6"><polygon points="13 2 3 14 12 14 11 22 21 10 12 10 13 2"/></svg>
                <svg v-else-if="stat.id === 3" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-6 h-6"><polygon points="5 3 19 12 5 21 5 3"/></svg>
                <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-6 h-6"><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/></svg>
              </div>
              <div 
                class="flex items-center gap-1 px-2.5 py-1 rounded-full text-xs font-semibold"
                :class="stat.trendType === 'up' ? 'bg-emerald-500/10 text-emerald-600' : 'bg-red-500/10 text-red-600'"
              >
                <svg v-if="stat.trendType === 'up'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-3 h-3"><polyline points="23 6 13.5 15.5 8.5 10.5 1 18"/></svg>
                <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-3 h-3"><polyline points="23 18 13.5 8.5 8.5 13.5 1 6"/></svg>
                {{ stat.trend }}
              </div>
            </div>
            <div class="text-3xl font-bold text-slate-800 mb-1 tracking-tight">{{ stat.value }}</div>
            <div class="text-sm text-slate-500">{{ stat.label }}</div>
          </div>
        </div>

        <!-- Content Grid -->
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 mb-8">
          <!-- Recent Properties -->
          <div class="bg-white rounded-2xl shadow-sm border border-slate-100 overflow-hidden">
            <div class="flex justify-between items-center px-6 py-5 border-b border-slate-100">
              <h3 class="font-bold text-slate-800">ทรัพย์ล่าสุด</h3>
              <NuxtLink to="/admin/properties" class="text-sm font-semibold text-[#DAA520] hover:underline">ดูทั้งหมด →</NuxtLink>
            </div>
            <div class="divide-y divide-slate-50">
              <div v-for="property in recentProperties" :key="property.id" class="flex items-center gap-4 p-4 hover:bg-slate-50 transition-colors">
                <img :src="property.image" :alt="property.title" class="w-16 h-12 rounded-lg object-cover" />
                <div class="flex-1 min-w-0">
                  <h4 class="font-semibold text-slate-800 truncate">{{ property.title }}</h4>
                  <p class="text-xs text-slate-500 mb-1">{{ property.location }}</p>
                  <span class="text-sm font-bold text-[#DAA520]">฿{{ formatPrice(property.price) }}</span>
                </div>
                <span 
                  class="px-2.5 py-1 rounded-md text-xs font-semibold"
                  :class="{
                    'bg-emerald-500/10 text-emerald-600': property.status === 'active',
                    'bg-amber-500/10 text-amber-600': property.status === 'pending',
                    'bg-indigo-500/10 text-indigo-600': property.status === 'sold'
                  }"
                >
                  {{ property.statusText }}
                </span>
              </div>
            </div>
          </div>

          <!-- Recent Content -->
          <div class="bg-white rounded-2xl shadow-sm border border-slate-100 overflow-hidden">
            <div class="flex justify-between items-center px-6 py-5 border-b border-slate-100">
              <h3 class="font-bold text-slate-800">คอนเทนต์ล่าสุด</h3>
              <NuxtLink to="/admin/content" class="text-sm font-semibold text-[#DAA520] hover:underline">ดูทั้งหมด →</NuxtLink>
            </div>
            <div class="divide-y divide-slate-50">
              <div v-for="item in recentContent" :key="item.id" class="flex items-center gap-4 p-4 hover:bg-slate-50 transition-colors">
                <div 
                  class="w-12 h-12 rounded-xl flex items-center justify-center flex-shrink-0"
                  :class="item.type === 'video' ? 'bg-red-500/10 text-red-500' : 'bg-indigo-500/10 text-indigo-500'"
                >
                  <svg v-if="item.type === 'video'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><polygon points="5 3 19 12 5 21 5 3"/></svg>
                  <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/></svg>
                </div>
                <div class="flex-1 min-w-0">
                  <h4 class="font-semibold text-slate-800 truncate">{{ item.title }}</h4>
                  <p class="text-xs text-slate-500">{{ item.typeText }} • {{ item.date }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Users and Activity -->
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
          <!-- Team Members -->
          <div class="bg-white rounded-2xl shadow-sm border border-slate-100 overflow-hidden">
            <div class="flex justify-between items-center px-6 py-5 border-b border-slate-100">
              <h3 class="font-bold text-slate-800">ผู้ใช้งานล่าสุด</h3>
              <NuxtLink to="/admin/users" class="text-sm font-semibold text-[#DAA520] hover:underline">ดูทั้งหมด →</NuxtLink>
            </div>
            <div class="divide-y divide-slate-50">
              <div v-for="user in recentUsers" :key="user.id" class="flex items-center gap-4 p-4 hover:bg-slate-50 transition-colors">
                <img :src="user.avatar" :alt="user.name" class="w-12 h-12 rounded-xl object-cover" />
                <div class="flex-1">
                  <div class="font-semibold text-slate-800">{{ user.name }}</div>
                  <div class="text-xs text-slate-500">{{ user.role }}</div>
                </div>
                <span 
                  class="px-2.5 py-1 rounded-md text-xs font-semibold"
                  :class="user.status === 'active' ? 'bg-emerald-500/10 text-emerald-600' : 'bg-slate-100 text-slate-500'"
                >
                  {{ user.status === 'active' ? 'Active' : 'Offline' }}
                </span>
              </div>
            </div>
          </div>

          <!-- Quick Stats -->
          <div class="bg-white rounded-2xl shadow-sm border border-slate-100 overflow-hidden">
            <div class="px-6 py-5 border-b border-slate-100">
              <h3 class="font-bold text-slate-800">สรุปภาพรวม</h3>
            </div>
            <div class="grid grid-cols-3 gap-4 p-6">
              <div class="bg-slate-50 rounded-xl p-4 text-center">
                <span class="block text-xs text-slate-500 mb-2">ทรัพย์ทั้งหมด</span>
                <span class="text-xl font-bold text-slate-800">1,248</span>
              </div>
              <div class="bg-slate-50 rounded-xl p-4 text-center">
                <span class="block text-xs text-slate-500 mb-2">Hot Deals</span>
                <span class="text-xl font-bold text-amber-500">45</span>
              </div>
              <div class="bg-slate-50 rounded-xl p-4 text-center">
                <span class="block text-xs text-slate-500 mb-2">วิดีโอ</span>
                <span class="text-xl font-bold text-slate-800">28</span>
              </div>
              <div class="bg-slate-50 rounded-xl p-4 text-center">
                <span class="block text-xs text-slate-500 mb-2">บทความ</span>
                <span class="text-xl font-bold text-slate-800">56</span>
              </div>
              <div class="bg-slate-50 rounded-xl p-4 text-center">
                <span class="block text-xs text-slate-500 mb-2">Admin</span>
                <span class="text-xl font-bold text-slate-800">2</span>
              </div>
              <div class="bg-slate-50 rounded-xl p-4 text-center">
                <span class="block text-xs text-slate-500 mb-2">Editor</span>
                <span class="text-xl font-bold text-slate-800">5</span>
              </div>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import AdminSidebar from '~/components/admin/AdminSidebar.vue'
import AdminHeader from '~/components/admin/AdminHeader.vue'

definePageMeta({
  layout: false
})

const isSidebarCollapsed = ref(false)
const isMobileMenuOpen = ref(false)

const toggleSidebar = () => {
  isSidebarCollapsed.value = !isSidebarCollapsed.value
}

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false
}

const greeting = computed(() => {
  const hour = new Date().getHours()
  if (hour < 12) return 'ตอนเช้า'
  if (hour < 18) return 'ตอนบ่าย'
  return 'ตอนเย็น'
})

const stats = [
  { id: 1, label: 'ทรัพย์ทั้งหมด', value: '1,248', trend: '+12 ใหม่', trendType: 'up', color: 'gold' },
  { id: 2, label: 'Hot Deals', value: '45', trend: '+5 สัปดาห์นี้', trendType: 'up', color: 'orange' },
  { id: 3, label: 'Content', value: '84', trend: '+3 ใหม่', trendType: 'up', color: 'purple' },
  { id: 4, label: 'ผู้ใช้งาน', value: '7', trend: 'Active 5', trendType: 'up', color: 'green' }
]

const recentProperties = [
  { id: 1, title: 'Ideo Q สุขุมวิท 36', location: 'สุขุมวิท, กรุงเทพฯ', price: 4990000, image: 'https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=100&h=80&fit=crop', status: 'active', statusText: 'Active' },
  { id: 2, title: 'The Line พหลโยธิน', location: 'พหลโยธิน, กรุงเทพฯ', price: 6500000, image: 'https://images.unsplash.com/photo-1560448204-e02f11c3d0e2?w=100&h=80&fit=crop', status: 'pending', statusText: 'รออนุมัติ' },
  { id: 3, title: 'Noble Ploenchit', location: 'เพลินจิต, กรุงเทพฯ', price: 12500000, image: 'https://images.unsplash.com/photo-1600607687939-ce8a6c25118c?w=100&h=80&fit=crop', status: 'sold', statusText: 'ขายแล้ว' }
]

const recentContent = [
  { id: 1, title: 'รีวิวคอนโด Ideo Q สุขุมวิท 36', type: 'video', typeText: 'วิดีโอ', date: '28 ธ.ค. 2024' },
  { id: 2, title: '10 เคล็ดลับเลือกซื้อคอนโดสำหรับมือใหม่', type: 'article', typeText: 'บทความ', date: '25 ธ.ค. 2024' },
  { id: 3, title: 'พาชมโครงการ The Line พหลโยธิน', type: 'video', typeText: 'วิดีโอ', date: '22 ธ.ค. 2024' },
  { id: 4, title: 'ตลาดอสังหาฯ ปี 2025 แนวโน้มเป็นอย่างไร', type: 'article', typeText: 'บทความ', date: '20 ธ.ค. 2024' }
]

const recentUsers = [
  { id: 1, name: 'คุณสมชาย รุ่งเรือง', role: 'Admin', avatar: 'https://randomuser.me/api/portraits/men/32.jpg', status: 'active' },
  { id: 2, name: 'คุณนภา สุขใจ', role: 'Editor', avatar: 'https://randomuser.me/api/portraits/women/44.jpg', status: 'active' },
  { id: 3, name: 'คุณวิภา พัฒนา', role: 'Editor', avatar: 'https://randomuser.me/api/portraits/women/68.jpg', status: 'inactive' }
]

const formatPrice = (price) => new Intl.NumberFormat('th-TH').format(price)
</script>

<style scoped>
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.animate-fade-in {
  animation: fadeIn 0.4s ease-out forwards;
}
</style>

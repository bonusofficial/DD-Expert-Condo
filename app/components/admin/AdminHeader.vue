<template>
  <header class="sticky top-0 z-20 bg-white border-b border-slate-200 px-4 py-3 lg:px-8 lg:py-4 flex items-center justify-between shadow-sm">
    <div class="flex items-center gap-4">
      <!-- Mobile Menu Button -->
      <button 
        class="lg:hidden p-2 -ml-2 rounded-lg text-slate-600 hover:bg-slate-100 transition-colors"
        @click="$emit('toggle-mobile')"
      >
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-6 h-6">
          <line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="18" x2="21" y2="18"/>
        </svg>
      </button>

      <!-- Desktop Collapse Button -->
      <button 
        class="hidden lg:flex p-2 rounded-lg text-slate-400 hover:text-slate-600 hover:bg-slate-100 transition-colors"
        @click="$emit('toggle-collapse')"
      >
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5">
          <line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="18" x2="21" y2="18"/>
        </svg>
      </button>

      <!-- Page Info -->
      <div class="flex flex-col">
        <h1 class="text-lg lg:text-xl font-bold text-slate-800 leading-tight">{{ pageTitle }}</h1>
        <nav class="hidden sm:flex items-center gap-2 text-xs text-slate-400">
          <NuxtLink to="/admin" class="hover:text-[#DAA520] transition-colors">Admin</NuxtLink>
          <span v-if="currentPage !== 'dashboard'">/</span>
          <span v-if="currentPage !== 'dashboard'" class="text-slate-500">{{ currentPageName }}</span>
        </nav>
      </div>
    </div>

    <div class="flex items-center gap-3 lg:gap-4">
      <!-- Search Box -->
      <div class="hidden md:flex items-center gap-2 bg-slate-100 px-4 py-2.5 rounded-xl w-64 focus-within:ring-2 focus-within:ring-[#DAA520]/20 focus-within:bg-white transition-all">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4 text-slate-400">
          <circle cx="11" cy="11" r="8"/><path d="m21 21-4.35-4.35"/>
        </svg>
        <input 
          type="text" 
          placeholder="ค้นหา..." 
          class="bg-transparent border-none outline-none text-sm text-slate-700 w-full placeholder:text-slate-400"
        />
      </div>

      <!-- Notifications -->
      <button class="relative p-2.5 rounded-xl bg-slate-50 text-slate-500 hover:bg-slate-100 hover:text-slate-700 transition-colors">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5">
          <path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/>
        </svg>
        <span class="absolute top-2 right-2 w-2 h-2 bg-red-500 rounded-full border-2 border-white"></span>
      </button>

      <!-- Go to Site -->
      <NuxtLink 
        to="/" 
        class="p-2.5 rounded-xl bg-slate-50 text-slate-500 hover:bg-slate-100 hover:text-[#DAA520] transition-colors"
        title="ไปยังหน้าเว็บ"
      >
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5">
          <circle cx="12" cy="12" r="10"/><line x1="2" y1="12" x2="22" y2="12"/><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1 4-10z"/>
        </svg>
      </NuxtLink>
    </div>
  </header>
</template>

<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'

defineEmits(['toggle-mobile', 'toggle-collapse'])

const route = useRoute()
const currentRoute = computed(() => route.path)

const currentPage = computed(() => {
  const path = currentRoute.value
  if (path === '/admin') return 'dashboard'
  if (path.includes('/properties')) return 'properties'
  if (path.includes('/content')) return 'content'
  if (path.includes('/users')) return 'users'
  if (path.includes('/settings')) return 'settings'
  if (path.includes('/menu-footer')) return 'menu-footer'
  return 'dashboard'
})

const pageTitle = computed(() => {
  const titles = {
    dashboard: 'Dashboard',
    properties: 'จัดการทรัพย์',
    content: 'Banner Management',
    users: 'จัดการผู้ใช้',
    settings: 'ตั้งค่าระบบ',
    'menu-footer': 'Menu & Footer Management'
  }
  return titles[currentPage.value] || 'Dashboard'
})

const currentPageName = computed(() => {
  const names = {
    properties: 'ทรัพย์',
    content: 'แบนเนอร์',
    users: 'ผู้ใช้',
    settings: 'ตั้งค่า',
    'menu-footer': 'Menu & Footer'
  }
  return names[currentPage.value] || ''
})
</script>

<template>
  <aside 
    class="fixed left-0 top-0 bottom-0 z-40 flex flex-col bg-slate-900 text-white transition-all duration-300 ease-in-out shadow-xl"
    :class="[
      collapsed ? 'w-20' : 'w-[280px]',
      mobileOpen ? 'translate-x-0' : '-translate-x-full lg:translate-x-0'
    ]"
  >
    <!-- Logo Section -->
    <div 
      class="flex items-center justify-between px-5 py-6 border-b border-white/10"
      :class="{ 'justify-center px-3': collapsed }"
    >
      <div class="flex items-center gap-3 overflow-hidden">
        <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-[#DAA520] to-[#b8860b] flex items-center justify-center flex-shrink-0 shadow-lg shadow-amber-500/20">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-6 h-6 text-white">
            <path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/>
          </svg>
        </div>
        <span 
          v-if="!collapsed" 
          class="text-xl font-bold text-white tracking-tight whitespace-nowrap transition-opacity duration-300"
        >
          DDexpert
        </span>
      </div>
      
      <button 
        v-if="!collapsed"
        @click="$emit('toggle-collapse')"
        class="w-8 h-8 flex items-center justify-center rounded-lg bg-white/5 text-white/50 hover:bg-white/10 hover:text-white transition-colors lg:flex hidden"
      >
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5">
          <polyline points="15 18 9 12 15 6"/>
        </svg>
      </button>
    </div>

    <!-- Navigation -->
    <nav class="flex-1 overflow-y-auto py-6 px-4 space-y-8 scrollbar-thin scrollbar-thumb-white/10 scrollbar-track-transparent">
      <!-- Main Menu -->
      <div class="space-y-2">
        <div 
          v-if="!collapsed" 
          class="px-3 mb-3 text-xs font-semibold text-white/40 uppercase tracking-wider"
        >
          เมนูหลัก
        </div>
        
        <NuxtLink 
          to="/admin" 
          class="group flex items-center gap-3 px-4 py-3 rounded-xl transition-all duration-200"
          :class="isActive('/admin', true) ? 'bg-amber-500/10 text-[#DAA520]' : 'text-slate-400 hover:bg-white/5 hover:text-white'"
        >
          <div class="relative flex items-center justify-center w-6 h-6">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5 transition-transform group-hover:scale-110">
              <rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/>
              <rect x="3" y="14" width="7" height="7" rx="1"/><rect x="14" y="14" width="7" height="7" rx="1"/>
            </svg>
            <div v-if="isActive('/admin', true) && collapsed" class="absolute -right-2 top-1/2 -translate-y-1/2 w-1 h-8 bg-[#DAA520] rounded-l-full"></div>
          </div>
          <span v-if="!collapsed" class="font-medium whitespace-nowrap">Dashboard</span>
          
          <!-- Tooltip for collapsed state -->
          <div v-if="collapsed" class="absolute left-full ml-4 px-3 py-2 bg-slate-800 text-white text-xs rounded-md opacity-0 group-hover:opacity-100 pointer-events-none transition-opacity z-50 whitespace-nowrap shadow-xl">
            Dashboard
          </div>
        </NuxtLink>

        <NuxtLink 
          to="/admin/properties" 
          class="group flex items-center gap-3 px-4 py-3 rounded-xl transition-all duration-200"
          :class="isActive('/admin/properties') ? 'bg-amber-500/10 text-[#DAA520]' : 'text-slate-400 hover:bg-white/5 hover:text-white'"
        >
          <div class="relative flex items-center justify-center w-6 h-6">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5 transition-transform group-hover:scale-110">
              <path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/>
            </svg>
            <div v-if="isActive('/admin/properties') && collapsed" class="absolute -right-2 top-1/2 -translate-y-1/2 w-1 h-8 bg-[#DAA520] rounded-l-full"></div>
          </div>
          <span v-if="!collapsed" class="font-medium whitespace-nowrap">จัดการทรัพย์</span>
          
          <div v-if="collapsed" class="absolute left-full ml-4 px-3 py-2 bg-slate-800 text-white text-xs rounded-md opacity-0 group-hover:opacity-100 pointer-events-none transition-opacity z-50 whitespace-nowrap shadow-xl">
            จัดการทรัพย์
          </div>
        </NuxtLink>

        <NuxtLink 
          to="/admin/content" 
          class="group flex items-center gap-3 px-4 py-3 rounded-xl transition-all duration-200"
          :class="isActive('/admin/content') ? 'bg-amber-500/10 text-[#DAA520]' : 'text-slate-400 hover:bg-white/5 hover:text-white'"
        >
          <div class="relative flex items-center justify-center w-6 h-6">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5 transition-transform group-hover:scale-110">
              <polygon points="5 3 19 12 5 21 5 3"/>
            </svg>
            <div v-if="isActive('/admin/content') && collapsed" class="absolute -right-2 top-1/2 -translate-y-1/2 w-1 h-8 bg-[#DAA520] rounded-l-full"></div>
          </div>
          <span v-if="!collapsed" class="font-medium whitespace-nowrap">จัดการแบนเนอร์</span>
          
          <div v-if="collapsed" class="absolute left-full ml-4 px-3 py-2 bg-slate-800 text-white text-xs rounded-md opacity-0 group-hover:opacity-100 pointer-events-none transition-opacity z-50 whitespace-nowrap shadow-xl">
            จัดการแบนเนอร์
          </div>
        </NuxtLink>

        <NuxtLink 
          to="/admin/menu-footer" 
          class="group flex items-center gap-3 px-4 py-3 rounded-xl transition-all duration-200"
          :class="isActive('/admin/menu-footer') ? 'bg-amber-500/10 text-[#DAA520]' : 'text-slate-400 hover:bg-white/5 hover:text-white'"
        >
          <div class="relative flex items-center justify-center w-6 h-6">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5 transition-transform group-hover:scale-110">
              <rect x="3" y="3" width="18" height="18" rx="2" ry="2"/><line x1="3" y1="9" x2="21" y2="9"/><line x1="9" y1="21" x2="9" y2="9"/>
            </svg>
            <div v-if="isActive('/admin/menu-footer') && collapsed" class="absolute -right-2 top-1/2 -translate-y-1/2 w-1 h-8 bg-[#DAA520] rounded-l-full"></div>
          </div>
          <span v-if="!collapsed" class="font-medium whitespace-nowrap">จัดการเมนู & Footer</span>
          
          <div v-if="collapsed" class="absolute left-full ml-4 px-3 py-2 bg-slate-800 text-white text-xs rounded-md opacity-0 group-hover:opacity-100 pointer-events-none transition-opacity z-50 whitespace-nowrap shadow-xl">
            จัดการเมนู & Footer
          </div>
        </NuxtLink>

        <NuxtLink 
          to="/admin/users" 
          class="group flex items-center gap-3 px-4 py-3 rounded-xl transition-all duration-200"
          :class="isActive('/admin/users') ? 'bg-amber-500/10 text-[#DAA520]' : 'text-slate-400 hover:bg-white/5 hover:text-white'"
        >
          <div class="relative flex items-center justify-center w-6 h-6">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5 transition-transform group-hover:scale-110">
              <path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/>
              <path d="M22 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/>
            </svg>
            <div v-if="isActive('/admin/users') && collapsed" class="absolute -right-2 top-1/2 -translate-y-1/2 w-1 h-8 bg-[#DAA520] rounded-l-full"></div>
          </div>
          <span v-if="!collapsed" class="font-medium whitespace-nowrap">จัดการผู้ใช้</span>
          
          <div v-if="collapsed" class="absolute left-full ml-4 px-3 py-2 bg-slate-800 text-white text-xs rounded-md opacity-0 group-hover:opacity-100 pointer-events-none transition-opacity z-50 whitespace-nowrap shadow-xl">
            จัดการผู้ใช้
          </div>
        </NuxtLink>
      </div>

      <!-- Settings Menu -->
      <div class="space-y-2">
        <div 
          v-if="!collapsed" 
          class="px-3 mb-3 text-xs font-semibold text-white/40 uppercase tracking-wider"
        >
          ตั้งค่า
        </div>

        <NuxtLink 
          to="/admin/settings" 
          class="group flex items-center gap-3 px-4 py-3 rounded-xl transition-all duration-200"
          :class="isActive('/admin/settings') ? 'bg-amber-500/10 text-[#DAA520]' : 'text-slate-400 hover:bg-white/5 hover:text-white'"
        >
          <div class="relative flex items-center justify-center w-6 h-6">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5 transition-transform group-hover:scale-110">
              <circle cx="12" cy="12" r="3"/>
              <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"/>
            </svg>
            <div v-if="isActive('/admin/settings') && collapsed" class="absolute -right-2 top-1/2 -translate-y-1/2 w-1 h-8 bg-[#DAA520] rounded-l-full"></div>
          </div>
          <span v-if="!collapsed" class="font-medium whitespace-nowrap">ตั้งค่าระบบ</span>
          
          <div v-if="collapsed" class="absolute left-full ml-4 px-3 py-2 bg-slate-800 text-white text-xs rounded-md opacity-0 group-hover:opacity-100 pointer-events-none transition-opacity z-50 whitespace-nowrap shadow-xl">
            ตั้งค่าระบบ
          </div>
        </NuxtLink>
      </div>
    </nav>

    <!-- User Profile -->
    <div class="p-4 border-t border-white/10">
      <div 
        class="flex items-center gap-3 p-3 rounded-xl bg-white/5 hover:bg-white/10 transition-colors cursor-pointer"
        :class="{ 'justify-center p-2': collapsed }"
      >
        <img 
          src="https://randomuser.me/api/portraits/men/32.jpg" 
          alt="Admin" 
          class="w-10 h-10 rounded-lg object-cover ring-2 ring-white/10"
        />
        <div v-if="!collapsed" class="flex-1 min-w-0">
          <div class="text-sm font-semibold text-white truncate">Admin</div>
          <div class="text-xs text-white/50 truncate">Super Admin</div>
        </div>
        <button 
          v-if="!collapsed"
          class="p-2 rounded-lg text-white/50 hover:text-red-400 hover:bg-red-400/10 transition-colors"
          title="ออกจากระบบ"
        >
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5">
            <path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><polyline points="16 17 21 12 16 7"/><line x1="21" y1="12" x2="9" y2="12"/>
          </svg>
        </button>
      </div>
    </div>
  </aside>

  <!-- Mobile Overlay -->
  <div 
    v-if="mobileOpen"
    class="fixed inset-0 bg-black/50 backdrop-blur-sm z-30 lg:hidden"
    @click="$emit('close-mobile')"
  ></div>
</template>

<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'

const props = defineProps({
  collapsed: {
    type: Boolean,
    default: false
  },
  mobileOpen: {
    type: Boolean,
    default: false
  }
})

defineEmits(['toggle-collapse', 'close-mobile'])

const route = useRoute()
const currentRoute = computed(() => route.path)

const isActive = (path, exact = false) => {
  if (exact) {
    return currentRoute.value === path
  }
  return currentRoute.value.startsWith(path)
}
</script>

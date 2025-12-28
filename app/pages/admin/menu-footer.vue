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
        <!-- Page Header -->
        <div class="mb-8">
          <h1 class="text-2xl font-bold text-slate-800">Menu & Footer Management</h1>
          <p class="text-slate-500 mt-1">จัดการข้อมูลการติดต่อและส่วนท้ายของเว็บไซต์</p>
        </div>

        <div class="grid grid-cols-1 xl:grid-cols-3 gap-8">
          <!-- Left Column: Contact Info & Footer Text -->
          <div class="xl:col-span-2 space-y-8">
            <!-- Contact Information -->
            <div class="bg-white rounded-2xl shadow-sm border border-slate-100 p-6">
              <div class="flex items-center gap-3 mb-6 pb-4 border-b border-slate-100">
                <div class="w-10 h-10 rounded-lg bg-blue-50 flex items-center justify-center text-blue-600">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5">
                    <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/>
                  </svg>
                </div>
                <div>
                  <h2 class="text-lg font-bold text-slate-800">ข้อมูลการติดต่อ</h2>
                  <p class="text-sm text-slate-500">ข้อมูลที่แสดงในส่วน Header และ Footer</p>
                </div>
              </div>

              <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">เบอร์โทรศัพท์</label>
                  <input type="text" v-model="form.phone" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="0xx-xxx-xxxx" />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">อีเมล</label>
                  <input type="email" v-model="form.email" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="contact@example.com" />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">Line ID / Link</label>
                  <input type="text" v-model="form.line" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="@ddexpert" />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">Facebook Page URL</label>
                  <input type="text" v-model="form.facebook" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="https://facebook.com/..." />
                </div>
                <div class="col-span-1 md:col-span-2 space-y-2">
                  <label class="text-sm font-medium text-slate-700">ที่อยู่</label>
                  <textarea v-model="form.address" rows="3" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="ระบุที่อยู่บริษัท..."></textarea>
                </div>
              </div>
            </div>

            <!-- Footer Content -->
            <div class="bg-white rounded-2xl shadow-sm border border-slate-100 p-6">
              <div class="flex items-center gap-3 mb-6 pb-4 border-b border-slate-100">
                <div class="w-10 h-10 rounded-lg bg-purple-50 flex items-center justify-center text-purple-600">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5">
                    <rect x="3" y="3" width="18" height="18" rx="2" ry="2"/><line x1="3" y1="9" x2="21" y2="9"/><line x1="9" y1="21" x2="9" y2="9"/>
                  </svg>
                </div>
                <div>
                  <h2 class="text-lg font-bold text-slate-800">เนื้อหา Footer</h2>
                  <p class="text-sm text-slate-500">ข้อความและคำอธิบายด้านล่างเว็บไซต์</p>
                </div>
              </div>

              <div class="space-y-6">
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">คำอธิบายเกี่ยวกับเรา (Footer Description)</label>
                  <textarea v-model="form.footerDescription" rows="4" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="ข้อความสั้นๆ เกี่ยวกับบริษัทที่แสดงในส่วน Footer..."></textarea>
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">ข้อความลิขสิทธิ์ (Copyright)</label>
                  <input type="text" v-model="form.copyright" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="© 2024 DDexpert Condo. All rights reserved." />
                </div>
              </div>
            </div>
          </div>

          <!-- Right Column: Footer Links -->
          <div class="space-y-8">
            <div class="bg-white rounded-2xl shadow-sm border border-slate-100 p-6 h-full">
              <div class="flex items-center justify-between mb-6 pb-4 border-b border-slate-100">
                <div class="flex items-center gap-3">
                  <div class="w-10 h-10 rounded-lg bg-emerald-50 flex items-center justify-center text-emerald-600">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5">
                      <path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"/><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"/>
                    </svg>
                  </div>
                  <div>
                    <h2 class="text-lg font-bold text-slate-800">ลิงก์เมนู Footer</h2>
                    <p class="text-sm text-slate-500">จัดการลิงก์ทางลัด</p>
                  </div>
                </div>
                <button @click="addLink" class="p-2 rounded-lg bg-slate-50 text-slate-600 hover:bg-slate-100 hover:text-[#DAA520] transition-colors">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
                </button>
              </div>

              <div class="space-y-4">
                <div v-if="form.footerLinks.length === 0" class="text-center py-8 text-slate-400 text-sm bg-slate-50 rounded-xl border-2 border-dashed border-slate-200">
                  ยังไม่มีลิงก์
                </div>
                
                <div 
                  v-for="(link, index) in form.footerLinks" 
                  :key="index"
                  class="group p-4 rounded-xl bg-slate-50 border border-slate-100 hover:border-[#DAA520]/30 hover:shadow-sm transition-all"
                >
                  <div class="flex justify-between items-start mb-3">
                    <span class="text-xs font-bold text-slate-400 uppercase tracking-wider">Link #{{ index + 1 }}</span>
                    <button @click="removeLink(index)" class="text-slate-400 hover:text-red-500 transition-colors">
                      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
                    </button>
                  </div>
                  <div class="space-y-3">
                    <input type="text" v-model="link.label" class="w-full px-3 py-2 bg-white border border-slate-200 rounded-lg text-sm focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="ชื่อลิงก์ (เช่น หน้าแรก)" />
                    <input type="text" v-model="link.url" class="w-full px-3 py-2 bg-white border border-slate-200 rounded-lg text-sm focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="URL (เช่น /home)" />
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Save Button -->
        <div class="fixed bottom-6 right-6 lg:bottom-8 lg:right-8 z-30">
          <button 
            @click="saveData"
            class="flex items-center gap-2 px-6 py-3 rounded-full bg-[#DAA520] text-white font-bold shadow-lg shadow-amber-500/30 hover:bg-[#b8860b] hover:scale-105 hover:-translate-y-1 transition-all duration-300"
          >
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5">
              <path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"/><polyline points="17 21 17 13 7 13 7 21"/><polyline points="7 3 7 8 15 8"/>
            </svg>
            บันทึกการเปลี่ยนแปลง
          </button>
        </div>
      </main>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
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

// Form Data
const form = reactive({
  phone: '02-123-4567',
  email: 'contact@ddexpert.com',
  line: '@ddexpert',
  facebook: 'https://facebook.com/ddexpert',
  address: '123 อาคารเอ็มไพร์ทาวเวอร์ ชั้น 25 ถนนสาทรใต้ แขวงยานนาวา เขตสาทร กรุงเทพฯ 10120',
  footerDescription: 'DDexpert Condo ศูนย์รวมคอนโดมือสองคุณภาพดี ราคาคุ้มค่า คัดสรรมาเพื่อคุณ พร้อมบริการให้คำปรึกษาด้านสินเชื่อและการโอนกรรมสิทธิ์ครบวงจร',
  copyright: '© 2025 DDexpert Condo. All rights reserved.',
  footerLinks: [
    { label: 'หน้าแรก', url: '/' },
    { label: 'ค้นหาคอนโด', url: '/search' },
    { label: 'บทความ', url: '/articles' },
    { label: 'เกี่ยวกับเรา', url: '/about' },
    { label: 'ติดต่อเรา', url: '/contact' }
  ]
})

const addLink = () => {
  form.footerLinks.push({ label: '', url: '' })
}

const removeLink = (index) => {
  form.footerLinks.splice(index, 1)
}

const saveData = () => {
  // Simulate API call
  console.log('Saving data:', form)
  
  // Show success animation/notification
  const btn = document.activeElement
  if (btn) {
    const originalText = btn.innerHTML
    btn.innerHTML = `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><polyline points="20 6 9 17 4 12"/></svg> บันทึกแล้ว`
    btn.classList.add('bg-emerald-500', 'hover:bg-emerald-600', 'shadow-emerald-500/30')
    btn.classList.remove('bg-[#DAA520]', 'hover:bg-[#b8860b]', 'shadow-amber-500/30')
    
    setTimeout(() => {
      btn.innerHTML = originalText
      btn.classList.remove('bg-emerald-500', 'hover:bg-emerald-600', 'shadow-emerald-500/30')
      btn.classList.add('bg-[#DAA520]', 'hover:bg-[#b8860b]', 'shadow-amber-500/30')
    }, 2000)
  }
}
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

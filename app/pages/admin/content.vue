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
          <h1 class="text-2xl font-bold text-slate-800">Banner Management</h1>
          <p class="text-slate-500 mt-1">จัดการแบนเนอร์ประชาสัมพันธ์หน้าเว็บไซต์</p>
        </div>

        <!-- Banner List -->
        <div class="animate-fade-in">
          <div class="flex justify-between items-center mb-6">
            <h2 class="text-lg font-bold text-slate-800">รายการแบนเนอร์</h2>
            <button class="flex items-center gap-2 px-4 py-2.5 rounded-xl bg-[#DAA520] text-white hover:bg-[#b8860b] font-medium shadow-lg shadow-amber-500/20 transition-all hover:-translate-y-0.5 text-sm" @click="openModal">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4">
                <line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/>
              </svg>
              เพิ่มแบนเนอร์ใหม่
            </button>
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
            <div 
              v-for="(banner, index) in banners" 
              :key="banner.id" 
              class="group bg-white rounded-2xl border border-slate-100 overflow-hidden hover:shadow-xl hover:shadow-slate-200/50 hover:-translate-y-1 transition-all duration-300 relative"
              draggable="true" 
              @dragstart="dragStart($event, index)" 
              @dragover.prevent 
              @drop="drop($event, index)"
            >
              <!-- Drag Handle -->
              <div class="absolute top-3 left-3 w-8 h-8 bg-white/90 backdrop-blur-sm rounded-lg flex items-center justify-center cursor-grab opacity-0 group-hover:opacity-100 transition-opacity z-10 shadow-sm">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4 text-slate-500">
                  <circle cx="9" cy="5" r="1"/><circle cx="9" cy="12" r="1"/><circle cx="9" cy="19" r="1"/>
                  <circle cx="15" cy="5" r="1"/><circle cx="15" cy="12" r="1"/><circle cx="15" cy="19" r="1"/>
                </svg>
              </div>

              <!-- Status Badge -->
              <div class="absolute top-3 right-3 z-10">
                <span 
                  class="px-2 py-1 rounded-md text-[10px] font-bold uppercase tracking-wider backdrop-blur-sm shadow-sm"
                  :class="banner.isActive ? 'bg-emerald-500/90 text-white' : 'bg-slate-500/90 text-white'"
                >
                  {{ banner.isActive ? 'Active' : 'Inactive' }}
                </span>
              </div>

              <!-- Preview Area -->
              <div class="relative aspect-video bg-slate-100 flex items-center justify-center overflow-hidden">
                <!-- Image Type -->
                <img v-if="banner.type === 'image'" :src="banner.imageUrl" class="w-full h-full object-cover" />
                
                <!-- Video Type -->
                <video v-else-if="banner.type === 'video'" :src="banner.videoUrl" class="w-full h-full object-cover" muted loop autoplay></video>
                
                <!-- YouTube Type -->
                <div v-else-if="banner.type === 'youtube'" class="w-full h-full relative">
                  <img :src="getYouTubeThumbnail(banner.youtubeUrl)" class="w-full h-full object-cover" />
                  <div class="absolute inset-0 flex items-center justify-center bg-black/30">
                    <div class="w-12 h-12 rounded-full bg-red-600 flex items-center justify-center text-white shadow-lg">
                      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-5 h-5 ml-0.5"><polygon points="5 3 19 12 5 21 5 3"/></svg>
                    </div>
                  </div>
                </div>

                <!-- Type Indicator -->
                <div class="absolute bottom-2 left-2 px-2 py-1 bg-black/60 backdrop-blur-sm rounded-md text-white text-[10px] font-medium flex items-center gap-1.5">
                  <svg v-if="banner.type === 'image'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-3 h-3"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/></svg>
                  <svg v-else-if="banner.type === 'video'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-3 h-3"><rect x="2" y="2" width="20" height="20" rx="2.18" ry="2.18"/><line x1="7" y1="2" x2="7" y2="22"/><line x1="17" y1="2" x2="17" y2="22"/><line x1="2" y1="12" x2="22" y2="12"/><line x1="2" y1="7" x2="7" y2="7"/><line x1="2" y1="17" x2="7" y2="17"/><line x1="17" y1="17" x2="22" y2="17"/><line x1="17" y1="7" x2="22" y2="7"/></svg>
                  <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-3 h-3"><path d="M22.54 6.42a2.78 2.78 0 0 0-1.94-2C18.88 4 12 4 12 4s-6.88 0-8.6.46a2.78 2.78 0 0 0-1.94 2A29 29 0 0 0 1 11.75a29 29 0 0 0 .46 5.33A2.78 2.78 0 0 0 3.4 19c1.72.46 8.6.46 8.6.46s6.88 0 8.6-.46a2.78 2.78 0 0 0 1.94-2 29 29 0 0 0 .46-5.25 29 29 0 0 0-.46-5.33z"/><polygon points="9.75 15.02 15.5 11.75 9.75 8.48 9.75 15.02"/></svg>
                  {{ banner.type === 'image' ? 'Image' : banner.type === 'video' ? 'Video' : 'YouTube' }}
                </div>
              </div>

              <!-- Info -->
              <div class="p-4">
                <div class="flex items-center gap-2 mb-2">
                  <span class="px-2 py-0.5 rounded text-[10px] font-bold uppercase tracking-wider bg-amber-50 text-[#DAA520]">#{{ index + 1 }}</span>
                </div>
                <p class="text-xs text-slate-500 line-clamp-2 mb-3 h-8">{{ banner.description || 'ไม่มีคำอธิบาย' }}</p>
                
                <div class="flex items-center justify-between pt-3 border-t border-slate-50">
                  <div class="flex items-center gap-2">
                    <button 
                      class="text-xs font-medium px-2 py-1 rounded-md transition-colors"
                      :class="banner.isActive ? 'bg-emerald-50 text-emerald-600 hover:bg-emerald-100' : 'bg-slate-100 text-slate-500 hover:bg-slate-200'"
                      @click="toggleStatus(banner)"
                    >
                      {{ banner.isActive ? 'Active' : 'Disabled' }}
                    </button>
                  </div>
                  <div class="flex gap-1">
                    <button class="p-1.5 rounded-lg text-slate-400 hover:text-[#DAA520] hover:bg-amber-50 transition-colors" @click="editBanner(banner)">
                      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/></svg>
                    </button>
                    <button class="p-1.5 rounded-lg text-slate-400 hover:text-red-500 hover:bg-red-50 transition-colors" @click="deleteBanner(banner)">
                      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/></svg>
                    </button>
                  </div>
                </div>
              </div>
            </div>

            <!-- Add Card -->
            <div 
              class="bg-slate-50 rounded-2xl border-2 border-dashed border-slate-200 flex flex-col items-center justify-center min-h-[280px] cursor-pointer hover:border-[#DAA520] hover:bg-amber-50/10 transition-all group"
              @click="openModal"
            >
              <div class="w-16 h-16 rounded-full bg-white border border-slate-200 flex items-center justify-center mb-4 group-hover:scale-110 group-hover:border-[#DAA520] transition-all">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8 text-slate-400 group-hover:text-[#DAA520]"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
              </div>
              <span class="font-medium text-slate-500 group-hover:text-[#DAA520]">เพิ่มแบนเนอร์ใหม่</span>
            </div>
          </div>
        </div>
      </main>
    </div>

    <!-- Banner Modal -->
    <Teleport to="body">
      <div v-if="showModal" class="fixed inset-0 z-50 flex items-center justify-center p-4" @click.self="closeModal">
        <div class="absolute inset-0 bg-black/50 backdrop-blur-sm transition-opacity"></div>
        <div class="relative bg-white rounded-2xl shadow-2xl w-full max-w-lg animate-scale-in">
          <div class="flex justify-between items-center p-6 border-b border-slate-100">
            <h2 class="text-xl font-bold text-slate-800">{{ isEditing ? 'แก้ไขแบนเนอร์' : 'เพิ่มแบนเนอร์ใหม่' }}</h2>
            <button @click="closeModal" class="p-2 rounded-full hover:bg-slate-100 text-slate-400 hover:text-slate-600 transition-colors">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
            </button>
          </div>
          <div class="p-6 space-y-4">
            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">ประเภทแบนเนอร์</label>
              <div class="grid grid-cols-3 gap-3">
                <label 
                  class="flex flex-col items-center justify-center gap-2 p-3 rounded-xl border cursor-pointer transition-all"
                  :class="form.type === 'image' ? 'border-[#DAA520] bg-amber-50 text-[#DAA520]' : 'border-slate-200 hover:border-slate-300 text-slate-600'"
                >
                  <input type="radio" v-model="form.type" value="image" class="sr-only" />
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-6 h-6"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/></svg>
                  <span class="text-xs font-medium">รูปภาพ</span>
                </label>
                <label 
                  class="flex flex-col items-center justify-center gap-2 p-3 rounded-xl border cursor-pointer transition-all"
                  :class="form.type === 'video' ? 'border-[#DAA520] bg-amber-50 text-[#DAA520]' : 'border-slate-200 hover:border-slate-300 text-slate-600'"
                >
                  <input type="radio" v-model="form.type" value="video" class="sr-only" />
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-6 h-6"><rect x="2" y="2" width="20" height="20" rx="2.18" ry="2.18"/><line x1="7" y1="2" x2="7" y2="22"/><line x1="17" y1="2" x2="17" y2="22"/><line x1="2" y1="12" x2="22" y2="12"/><line x1="2" y1="7" x2="7" y2="7"/><line x1="2" y1="17" x2="7" y2="17"/><line x1="17" y1="17" x2="22" y2="17"/><line x1="17" y1="7" x2="22" y2="7"/></svg>
                  <span class="text-xs font-medium">วิดีโอ</span>
                </label>
                <label 
                  class="flex flex-col items-center justify-center gap-2 p-3 rounded-xl border cursor-pointer transition-all"
                  :class="form.type === 'youtube' ? 'border-[#DAA520] bg-amber-50 text-[#DAA520]' : 'border-slate-200 hover:border-slate-300 text-slate-600'"
                >
                  <input type="radio" v-model="form.type" value="youtube" class="sr-only" />
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-6 h-6"><path d="M22.54 6.42a2.78 2.78 0 0 0-1.94-2C18.88 4 12 4 12 4s-6.88 0-8.6.46a2.78 2.78 0 0 0-1.94 2A29 29 0 0 0 1 11.75a29 29 0 0 0 .46 5.33A2.78 2.78 0 0 0 3.4 19c1.72.46 8.6.46 8.6.46s6.88 0 8.6-.46a2.78 2.78 0 0 0 1.94-2 29 29 0 0 0 .46-5.25 29 29 0 0 0-.46-5.33z"/><polygon points="9.75 15.02 15.5 11.75 9.75 8.48 9.75 15.02"/></svg>
                  <span class="text-xs font-medium">YouTube</span>
                </label>
              </div>
            </div>
            <!-- Image Fields -->
            <div v-if="form.type === 'image'" class="space-y-4 animate-fade-in">
              <div class="space-y-2">
                <label class="text-sm font-medium text-slate-700">อัปโหลดรูปภาพ <span class="text-red-500">*</span></label>
                <div class="relative group">
                  <div v-if="form.imageUrl" class="relative w-full aspect-video rounded-xl overflow-hidden border border-slate-200 bg-slate-100">
                    <img :src="form.imageUrl" class="w-full h-full object-cover" />
                    <button @click="removeImage" class="absolute top-2 right-2 p-1.5 bg-red-500 text-white rounded-full hover:bg-red-600 transition-colors shadow-sm">
                      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
                    </button>
                  </div>
                  <label v-else class="flex flex-col items-center justify-center w-full h-48 border-2 border-slate-300 border-dashed rounded-xl cursor-pointer bg-slate-50 hover:bg-slate-100 hover:border-[#DAA520] transition-all">
                    <div class="flex flex-col items-center justify-center pt-5 pb-6">
                      <svg class="w-10 h-10 mb-3 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"></path></svg>
                      <p class="mb-1 text-sm text-slate-500"><span class="font-semibold text-[#DAA520]">คลิกเพื่ออัปโหลด</span> หรือลากไฟล์มาวาง</p>
                      <p class="text-xs text-slate-400">PNG, JPG (แนะนำ 1920x1080)</p>
                    </div>
                    <input type="file" class="hidden" accept="image/*" @change="handleImageUpload" />
                  </label>
                </div>
              </div>
            </div>

            <!-- Video Fields -->
            <div v-if="form.type === 'video'" class="space-y-4 animate-fade-in">
              <div class="space-y-2">
                <label class="text-sm font-medium text-slate-700">อัปโหลดวิดีโอ <span class="text-red-500">*</span></label>
                <div class="relative group">
                  <div v-if="form.videoUrl" class="relative w-full aspect-video rounded-xl overflow-hidden border border-slate-200 bg-slate-100">
                    <video :src="form.videoUrl" class="w-full h-full object-cover" controls></video>
                    <button @click="removeVideo" class="absolute top-2 right-2 p-1.5 bg-red-500 text-white rounded-full hover:bg-red-600 transition-colors shadow-sm z-10">
                      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
                    </button>
                  </div>
                  <label v-else class="flex flex-col items-center justify-center w-full h-48 border-2 border-slate-300 border-dashed rounded-xl cursor-pointer bg-slate-50 hover:bg-slate-100 hover:border-[#DAA520] transition-all">
                    <div class="flex flex-col items-center justify-center pt-5 pb-6">
                      <svg class="w-10 h-10 mb-3 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 10l4.553-2.276A1 1 0 0121 8.618v6.764a1 1 0 01-1.447.894L15 14M5 18h8a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v8a2 2 0 002 2z"></path></svg>
                      <p class="mb-1 text-sm text-slate-500"><span class="font-semibold text-[#DAA520]">คลิกเพื่ออัปโหลด</span> หรือลากไฟล์มาวาง</p>
                      <p class="text-xs text-slate-400">MP4, WebM (Max 10MB)</p>
                    </div>
                    <input type="file" class="hidden" accept="video/*" @change="handleVideoUpload" />
                  </label>
                </div>
              </div>
            </div>

            <!-- YouTube Fields -->
            <div v-if="form.type === 'youtube'" class="space-y-4 animate-fade-in">
              <div class="space-y-2">
                <label class="text-sm font-medium text-slate-700">YouTube URL <span class="text-red-500">*</span></label>
                <input type="text" v-model="form.youtubeUrl" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="https://www.youtube.com/watch?v=..." />
              </div>
              
              <!-- YouTube Preview -->
              <div v-if="form.youtubeUrl && getYouTubeThumbnail(form.youtubeUrl)" class="relative w-full aspect-video rounded-xl overflow-hidden border border-slate-200 bg-slate-100">
                <img :src="getYouTubeThumbnail(form.youtubeUrl)" class="w-full h-full object-cover" />
                <div class="absolute inset-0 flex items-center justify-center bg-black/30">
                  <div class="w-12 h-12 rounded-full bg-red-600 flex items-center justify-center text-white shadow-lg">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-5 h-5 ml-0.5"><polygon points="5 3 19 12 5 21 5 3"/></svg>
                  </div>
                </div>
              </div>
            </div>

            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">คำอธิบาย (Optional)</label>
              <textarea v-model="form.description" rows="2" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="รายละเอียดเพิ่มเติม..."></textarea>
            </div>

            <div class="flex items-center gap-2">
              <input type="checkbox" id="isActive" v-model="form.isActive" class="w-4 h-4 text-[#DAA520] border-slate-300 rounded focus:ring-[#DAA520]">
              <label for="isActive" class="text-sm text-slate-700">เปิดใช้งานทันที</label>
            </div>
          </div>
          <div class="p-6 border-t border-slate-100 flex justify-end gap-3 bg-slate-50 rounded-b-2xl">
            <button @click="closeModal" class="px-6 py-2.5 rounded-xl border border-slate-200 bg-white text-slate-600 hover:bg-slate-50 font-medium transition-colors">ยกเลิก</button>
            <button class="px-6 py-2.5 rounded-xl bg-[#DAA520] text-white hover:bg-[#b8860b] font-medium shadow-lg shadow-amber-500/20 transition-all" @click="saveBanner">
              {{ isEditing ? 'บันทึกการแก้ไข' : 'เพิ่มแบนเนอร์' }}
            </button>
          </div>
        </div>
      </div>
    </Teleport>
  </div>
</template>

<script setup>
import { ref } from 'vue'
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

// Banner State
const showModal = ref(false)
const isEditing = ref(false)
const draggedIndex = ref(null)

const defaultForm = {
  type: 'image', // image, video, youtube
  imageUrl: '',
  videoUrl: '',
  youtubeUrl: '',
  description: '',
  isActive: true
}

const form = ref({ ...defaultForm })

const banners = ref([
  {
    id: 1,
    type: 'image',
    imageUrl: 'https://images.unsplash.com/photo-1560448204-e02f11c3d0e2?w=800&h=450&fit=crop',
    description: 'ส่วนลดพิเศษสำหรับ 10 ยูนิตแรก',
    isActive: true
  },
  {
    id: 2,
    type: 'youtube',
    youtubeUrl: 'https://www.youtube.com/watch?v=dQw4w9WgXcQ',
    description: 'ชมห้องตัวอย่างแบบ 360 องศา',
    isActive: true
  },
  {
    id: 3,
    type: 'video',
    videoUrl: 'https://www.w3schools.com/html/mov_bbb.mp4',
    description: 'สระว่ายน้ำและฟิตเนสลอยฟ้า',
    isActive: false
  }
])

// Functions
const openModal = () => {
  isEditing.value = false
  form.value = { ...defaultForm }
  showModal.value = true
}

const editBanner = (banner) => {
  isEditing.value = true
  form.value = { ...banner }
  showModal.value = true
}

const closeModal = () => {
  showModal.value = false
}

const saveBanner = () => {
  if (form.value.type === 'image' && !form.value.imageUrl) {
    alert('กรุณาระบุ URL รูปภาพ')
    return
  }
  if (form.value.type === 'video' && !form.value.videoUrl) {
    alert('กรุณาระบุ URL วิดีโอ')
    return
  }
  if (form.value.type === 'youtube' && !form.value.youtubeUrl) {
    alert('กรุณาระบุ YouTube URL')
    return
  }
  
  if (isEditing.value) {
    const idx = banners.value.findIndex(b => b.id === form.value.id)
    if (idx !== -1) {
      banners.value[idx] = { ...form.value }
    }
  } else {
    const newId = Math.max(...banners.value.map(b => b.id), 0) + 1
    banners.value.push({
      ...form.value,
      id: newId
    })
  }
  
  closeModal()
}

const deleteBanner = (banner) => {
  if (confirm(`ต้องการลบแบนเนอร์ลำดับที่ ${banners.value.indexOf(banner) + 1} หรือไม่?`)) {
    const idx = banners.value.findIndex(b => b.id === banner.id)
    if (idx !== -1) {
      banners.value.splice(idx, 1)
    }
  }
}

const toggleStatus = (banner) => {
  banner.isActive = !banner.isActive
}

const getYouTubeThumbnail = (url) => {
  if (!url) return ''
  const regExp = /^.*(youtu.be\/|v\/|u\/\w\/|embed\/|watch\?v=|&v=)([^#&?]*).*/
  const match = url.match(regExp)
  const id = (match && match[2].length === 11) ? match[2] : null
  return id ? `https://img.youtube.com/vi/${id}/mqdefault.jpg` : ''
}

const handleImageUpload = (event) => {
  const file = event.target.files[0]
  if (file) {
    // Simulate upload by creating a local URL
    form.value.imageUrl = URL.createObjectURL(file)
  }
}

const removeImage = () => {
  form.value.imageUrl = ''
}

const handleVideoUpload = (event) => {
  const file = event.target.files[0]
  if (file) {
    // Simulate upload by creating a local URL
    form.value.videoUrl = URL.createObjectURL(file)
  }
}

const removeVideo = () => {
  form.value.videoUrl = ''
}

// Drag & Drop
const dragStart = (e, index) => {
  draggedIndex.value = index
}

const drop = (e, index) => {
  if (draggedIndex.value !== null && draggedIndex.value !== index) {
    const item = banners.value.splice(draggedIndex.value, 1)[0]
    banners.value.splice(index, 0, item)
  }
  draggedIndex.value = null
}
</script>

<style scoped>
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes scaleIn {
  from { opacity: 0; transform: scale(0.95); }
  to { opacity: 1; transform: scale(1); }
}

.animate-fade-in {
  animation: fadeIn 0.4s ease-out forwards;
}

.animate-scale-in {
  animation: scaleIn 0.3s ease-out forwards;
}
</style>

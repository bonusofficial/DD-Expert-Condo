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
        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 mb-8">
          <div>
            <h1 class="text-2xl font-bold text-slate-800">User Management</h1>
            <p class="text-slate-500 mt-1">จัดการผู้ใช้งานและสิทธิ์การเข้าถึงระบบ</p>
          </div>
          <button class="flex items-center gap-2 px-4 py-2.5 rounded-xl bg-gradient-to-r from-[#DAA520] to-[#b8860b] text-white hover:shadow-lg hover:shadow-amber-500/30 hover:-translate-y-0.5 transition-all duration-200 font-medium text-sm" @click="openUserModal">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4">
              <path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="8.5" cy="7" r="4"/>
              <line x1="20" y1="8" x2="20" y2="14"/><line x1="23" y1="11" x2="17" y2="11"/>
            </svg>
            เพิ่มผู้ใช้ใหม่
          </button>
        </div>

        <!-- Stats -->
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-8">
          <div v-for="(stat, index) in stats" :key="index" class="bg-white p-5 rounded-2xl shadow-sm border border-slate-100">
            <div class="flex justify-between items-start mb-2">
              <span class="text-slate-500 text-sm font-medium">{{ stat.label }}</span>
              <div class="w-8 h-8 rounded-lg flex items-center justify-center" :class="stat.bgClass">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4" :class="stat.textClass">
                  <path v-if="index === 0" d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle v-if="index === 0" cx="9" cy="7" r="4"/>
                  <path v-if="index === 1" d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/>
                  <circle v-if="index === 2" cx="12" cy="12" r="10"/><line v-if="index === 2" x1="12" y1="8" x2="12" y2="12"/><line v-if="index === 2" x1="12" y1="16" x2="12.01" y2="16"/>
                  <path v-if="index === 3" d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle v-if="index === 3" cx="8.5" cy="7" r="4"/><line v-if="index === 3" x1="23" y1="11" x2="17" y2="11"/>
                </svg>
              </div>
            </div>
            <div class="text-2xl font-bold text-slate-800">{{ stat.value }}</div>
          </div>
        </div>

        <!-- Filters -->
        <div class="bg-white p-4 rounded-2xl shadow-sm border border-slate-100 mb-6 flex flex-col lg:flex-row gap-4">
          <div class="flex-1 relative">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="absolute left-3 top-1/2 -translate-y-1/2 w-5 h-5 text-slate-400">
              <circle cx="11" cy="11" r="8"/><path d="m21 21-4.35-4.35"/>
            </svg>
            <input 
              type="text" 
              v-model="searchQuery" 
              placeholder="ค้นหาผู้ใช้งาน..." 
              class="w-full pl-10 pr-4 py-2.5 bg-slate-50 border-none rounded-xl text-slate-700 placeholder:text-slate-400 focus:ring-2 focus:ring-[#DAA520]/20 focus:bg-white transition-all outline-none"
            />
          </div>
          <div class="flex gap-3">
            <select v-model="filters.role" class="px-4 py-2.5 bg-slate-50 border-none rounded-xl text-slate-700 text-sm focus:ring-2 focus:ring-[#DAA520]/20 outline-none cursor-pointer min-w-[140px]">
              <option value="">ทุกตำแหน่ง</option>
              <option value="admin">Admin</option>
              <option value="editor">Editor</option>
              <option value="viewer">Viewer</option>
            </select>
            <select v-model="filters.status" class="px-4 py-2.5 bg-slate-50 border-none rounded-xl text-slate-700 text-sm focus:ring-2 focus:ring-[#DAA520]/20 outline-none cursor-pointer min-w-[140px]">
              <option value="">ทุกสถานะ</option>
              <option value="active">Active</option>
              <option value="inactive">Inactive</option>
            </select>
          </div>
        </div>

        <!-- Users Table -->
        <div class="bg-white rounded-2xl shadow-sm border border-slate-100 overflow-hidden">
          <div class="overflow-x-auto">
            <table class="w-full text-left border-collapse">
              <thead>
                <tr class="bg-slate-50 border-b border-slate-100 text-xs uppercase text-slate-500 font-semibold tracking-wider">
                  <th class="px-6 py-4 rounded-tl-2xl">ผู้ใช้งาน</th>
                  <th class="px-6 py-4">ตำแหน่ง</th>
                  <th class="px-6 py-4">สถานะ</th>
                  <th class="px-6 py-4">เข้าใช้งานล่าสุด</th>
                  <th class="px-6 py-4 text-right rounded-tr-2xl">จัดการ</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-slate-50">
                <tr v-for="user in filteredUsers" :key="user.id" class="hover:bg-slate-50/50 transition-colors group">
                  <td class="px-6 py-4">
                    <div class="flex items-center gap-4">
                      <img :src="user.avatar" :alt="user.name" class="w-10 h-10 rounded-full object-cover ring-2 ring-white shadow-sm" />
                      <div>
                        <div class="font-semibold text-slate-800 text-sm">{{ user.name }}</div>
                        <div class="text-xs text-slate-500">{{ user.email }}</div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4">
                    <span 
                      class="inline-flex items-center px-2.5 py-1 rounded-md text-xs font-medium"
                      :class="{
                        'bg-purple-50 text-purple-600': user.role === 'admin',
                        'bg-blue-50 text-blue-600': user.role === 'editor',
                        'bg-slate-100 text-slate-600': user.role === 'viewer'
                      }"
                    >
                      {{ user.roleText }}
                    </span>
                  </td>
                  <td class="px-6 py-4">
                    <span 
                      class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-semibold"
                      :class="{
                        'bg-emerald-50 text-emerald-600': user.status === 'active',
                        'bg-slate-100 text-slate-500': user.status === 'inactive'
                      }"
                    >
                      <span class="w-1.5 h-1.5 rounded-full mr-1.5" :class="{
                        'bg-emerald-500': user.status === 'active',
                        'bg-slate-400': user.status === 'inactive'
                      }"></span>
                      {{ user.status === 'active' ? 'Active' : 'Inactive' }}
                    </span>
                  </td>
                  <td class="px-6 py-4 text-sm text-slate-500">
                    {{ user.lastLogin }}
                  </td>
                  <td class="px-6 py-4 text-right">
                    <div class="flex items-center justify-end gap-2 opacity-0 group-hover:opacity-100 transition-opacity">
                      <button class="p-2 rounded-lg text-slate-400 hover:text-[#DAA520] hover:bg-amber-50 transition-colors" title="แก้ไข" @click="openUserModal(user)">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4">
                          <path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/>
                        </svg>
                      </button>
                      <button class="p-2 rounded-lg text-slate-400 hover:text-red-500 hover:bg-red-50 transition-colors" title="ลบ" @click="confirmDelete(user)">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4">
                          <polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/>
                        </svg>
                      </button>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </main>
    </div>

    <!-- Add/Edit Modal -->
    <Teleport to="body">
      <div v-if="showModal" class="fixed inset-0 z-50 flex items-center justify-center p-4" @click.self="closeModal">
        <div class="absolute inset-0 bg-black/50 backdrop-blur-sm transition-opacity"></div>
        <div class="relative bg-white rounded-2xl shadow-2xl w-full max-w-lg animate-scale-in">
          <div class="flex justify-between items-center p-6 border-b border-slate-100">
            <h2 class="text-xl font-bold text-slate-800">{{ isEditing ? 'แก้ไขข้อมูลผู้ใช้งาน' : 'เพิ่มผู้ใช้งานใหม่' }}</h2>
            <button @click="closeModal" class="p-2 rounded-full hover:bg-slate-100 text-slate-400 hover:text-slate-600 transition-colors">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
            </button>
          </div>
          <div class="p-6 space-y-4">
            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">ชื่อ-นามสกุล <span class="text-red-500">*</span></label>
              <input type="text" v-model="form.name" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="ระบุชื่อ-นามสกุล" />
            </div>
            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">อีเมล <span class="text-red-500">*</span></label>
              <input type="email" v-model="form.email" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="example@email.com" />
            </div>
            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">ตำแหน่ง <span class="text-red-500">*</span></label>
              <select v-model="form.role" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all">
                <option value="editor">Editor</option>
                <option value="admin">Admin</option>
              </select>
            </div>
            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">รหัสผ่าน {{ isEditing ? '(เว้นว่างหากไม่ต้องการเปลี่ยน)' : '*' }}</label>
              <input type="password" v-model="form.password" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="กำหนดรหัสผ่าน" />
            </div>
            <div class="flex items-center gap-2 pt-2">
              <input type="checkbox" id="status" v-model="form.isActive" class="w-4 h-4 text-[#DAA520] border-slate-300 rounded focus:ring-[#DAA520]">
              <label for="status" class="text-sm text-slate-700">เปิดใช้งาน (Active)</label>
            </div>
          </div>
          <div class="p-6 border-t border-slate-100 flex justify-end gap-3 bg-slate-50 rounded-b-2xl">
            <button @click="closeModal" class="px-6 py-2.5 rounded-xl border border-slate-200 bg-white text-slate-600 hover:bg-slate-50 font-medium transition-colors">ยกเลิก</button>
            <button @click="saveUser" class="px-6 py-2.5 rounded-xl bg-[#DAA520] text-white hover:bg-[#b8860b] font-medium shadow-lg shadow-amber-500/20 transition-all">
              {{ isEditing ? 'บันทึกการแก้ไข' : 'เพิ่มผู้ใช้งาน' }}
            </button>
          </div>
        </div>
      </div>
    </Teleport>
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

// User Management State
const showModal = ref(false)
const isEditing = ref(false)
const searchQuery = ref('')
const filters = ref({
  role: '',
  status: ''
})

const defaultForm = {
  id: null,
  name: '',
  email: '',
  role: 'editor',
  password: '',
  isActive: true
}

const form = ref({ ...defaultForm })

const stats = [
  { label: 'ผู้ใช้งานทั้งหมด', value: '4', bgClass: 'bg-blue-50', textClass: 'text-blue-600' },
  { label: 'Admin', value: '1', bgClass: 'bg-purple-50', textClass: 'text-purple-600' },
  { label: 'Active Now', value: '3', bgClass: 'bg-emerald-50', textClass: 'text-emerald-600' },
  { label: 'Inactive', value: '1', bgClass: 'bg-slate-100', textClass: 'text-slate-500' }
]

const users = ref([
  { 
    id: 1, 
    name: 'คุณสมชาย รุ่งเรือง', 
    email: 'somchai@example.com',
    role: 'admin', 
    roleText: 'Admin',
    avatar: 'https://randomuser.me/api/portraits/men/32.jpg', 
    status: 'active',
    lastLogin: '10 นาทีที่แล้ว'
  },
  { 
    id: 2, 
    name: 'คุณนภา สุขใจ', 
    email: 'napa@example.com',
    role: 'editor', 
    roleText: 'Editor',
    avatar: 'https://randomuser.me/api/portraits/women/44.jpg', 
    status: 'active',
    lastLogin: '1 ชั่วโมงที่แล้ว'
  },
  { 
    id: 3, 
    name: 'คุณวิภา พัฒนา', 
    email: 'wipa@example.com',
    role: 'editor', 
    roleText: 'Editor',
    avatar: 'https://randomuser.me/api/portraits/women/68.jpg', 
    status: 'inactive',
    lastLogin: '2 วันที่แล้ว'
  },
  { 
    id: 4, 
    name: 'คุณกิตติ มีทรัพย์', 
    email: 'kitti@example.com',
    role: 'editor', 
    roleText: 'Editor',
    avatar: 'https://randomuser.me/api/portraits/men/45.jpg', 
    status: 'active',
    lastLogin: '3 ชั่วโมงที่แล้ว'
  }
])

const filteredUsers = computed(() => {
  return users.value.filter(u => {
    if (searchQuery.value && !u.name.toLowerCase().includes(searchQuery.value.toLowerCase()) && !u.email.toLowerCase().includes(searchQuery.value.toLowerCase())) return false
    if (filters.value.role && u.role !== filters.value.role) return false
    if (filters.value.status && u.status !== filters.value.status) return false
    return true
  })
})

const openUserModal = (user = null) => {
  if (user) {
    isEditing.value = true
    form.value = { 
      id: user.id,
      name: user.name,
      email: user.email,
      role: user.role,
      password: '', // Don't show password
      isActive: user.status === 'active'
    }
  } else {
    isEditing.value = false
    form.value = { ...defaultForm }
  }
  showModal.value = true
}

const closeModal = () => {
  showModal.value = false
}

const saveUser = () => {
  if (!form.value.name || !form.value.email) {
    alert('กรุณากรอกข้อมูลให้ครบถ้วน')
    return
  }

  if (!isEditing.value && !form.value.password) {
    alert('กรุณากำหนดรหัสผ่าน')
    return
  }

  const userData = {
    name: form.value.name,
    email: form.value.email,
    role: form.value.role,
    roleText: form.value.role.charAt(0).toUpperCase() + form.value.role.slice(1),
    status: form.value.isActive ? 'active' : 'inactive',
    avatar: `https://ui-avatars.com/api/?name=${encodeURIComponent(form.value.name)}&background=random`,
    lastLogin: 'Just now'
  }

  if (isEditing.value) {
    const index = users.value.findIndex(u => u.id === form.value.id)
    if (index !== -1) {
      // Keep existing avatar and lastLogin if editing
      users.value[index] = { 
        ...users.value[index],
        ...userData,
        avatar: users.value[index].avatar,
        lastLogin: users.value[index].lastLogin
      }
    }
  } else {
    const newId = Math.max(...users.value.map(u => u.id), 0) + 1
    users.value.push({
      id: newId,
      ...userData
    })
  }

  closeModal()
}

const confirmDelete = (user) => {
  if (confirm(`ต้องการลบผู้ใช้ ${user.name} ใช่หรือไม่?`)) {
    const index = users.value.findIndex(u => u.id === user.id)
    if (index !== -1) {
      users.value.splice(index, 1)
    }
  }
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

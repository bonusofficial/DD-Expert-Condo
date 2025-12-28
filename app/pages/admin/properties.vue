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
            <h1 class="text-2xl font-bold text-slate-800">Property Management</h1>
            <p class="text-slate-500 mt-1">จัดการทรัพย์สินทั้งหมดของคุณ</p>
          </div>
          <div class="flex items-center gap-3">
            <button class="flex items-center gap-2 px-4 py-2.5 rounded-xl border border-slate-200 bg-white text-slate-600 hover:bg-slate-50 hover:text-slate-800 transition-colors font-medium text-sm" @click="exportProperties">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4">
                <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/>
              </svg>
              Export
            </button>
            <button class="flex items-center gap-2 px-4 py-2.5 rounded-xl bg-gradient-to-r from-[#DAA520] to-[#b8860b] text-white hover:shadow-lg hover:shadow-amber-500/30 hover:-translate-y-0.5 transition-all duration-200 font-medium text-sm" @click="openAddModal">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4">
                <line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/>
              </svg>
              เพิ่มทรัพย์ใหม่
            </button>
          </div>
        </div>

        <!-- Stats -->
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-8">
          <div v-for="(stat, index) in stats" :key="index" class="bg-white p-5 rounded-2xl shadow-sm border border-slate-100">
            <div class="flex justify-between items-start mb-2">
              <span class="text-slate-500 text-sm font-medium">{{ stat.label }}</span>
              <span :class="['text-xs font-semibold px-2 py-1 rounded-md', stat.trend > 0 ? 'bg-emerald-50 text-emerald-600' : 'bg-red-50 text-red-600']">
                {{ stat.trend > 0 ? '+' : '' }}{{ stat.trend }}%
              </span>
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
              placeholder="ค้นหาทรัพย์..." 
              class="w-full pl-10 pr-4 py-2.5 bg-slate-50 border-none rounded-xl text-slate-700 placeholder:text-slate-400 focus:ring-2 focus:ring-[#DAA520]/20 focus:bg-white transition-all outline-none"
            />
          </div>
          <div class="flex gap-3 overflow-x-auto pb-2 lg:pb-0">
            <select v-model="filters.type" class="px-4 py-2.5 bg-slate-50 border-none rounded-xl text-slate-700 text-sm focus:ring-2 focus:ring-[#DAA520]/20 outline-none cursor-pointer min-w-[140px]">
              <option value="">ทุกประเภท</option>
              <option value="condo">คอนโด</option>
              <option value="house">บ้านเดี่ยว</option>
              <option value="land">ที่ดิน</option>
            </select>
            <select v-model="filters.status" class="px-4 py-2.5 bg-slate-50 border-none rounded-xl text-slate-700 text-sm focus:ring-2 focus:ring-[#DAA520]/20 outline-none cursor-pointer min-w-[140px]">
              <option value="">ทุกสถานะ</option>
              <option value="active">Active</option>
              <option value="pending">Pending</option>
              <option value="sold">Sold</option>
            </select>
            <select v-model="filters.price" class="px-4 py-2.5 bg-slate-50 border-none rounded-xl text-slate-700 text-sm focus:ring-2 focus:ring-[#DAA520]/20 outline-none cursor-pointer min-w-[140px]">
              <option value="">ทุกช่วงราคา</option>
              <option value="low">ต่ำกว่า 3M</option>
              <option value="mid">3M - 10M</option>
              <option value="high">มากกว่า 10M</option>
            </select>
          </div>
        </div>

        <!-- Properties Table -->
        <div class="bg-white rounded-2xl shadow-sm border border-slate-100 overflow-hidden">
          <div class="overflow-x-auto">
            <table class="w-full text-left border-collapse">
              <thead>
                <tr class="bg-slate-50 border-b border-slate-100 text-xs uppercase text-slate-500 font-semibold tracking-wider">
                  <th class="px-6 py-4 rounded-tl-2xl">ทรัพย์สิน</th>
                  <th class="px-6 py-4">ประเภท</th>
                  <th class="px-6 py-4">ราคา</th>
                  <th class="px-6 py-4">สถานะ</th>
                  <th class="px-6 py-4">วันที่ลง</th>
                  <th class="px-6 py-4 text-right rounded-tr-2xl">จัดการ</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-slate-50">
                <tr v-for="property in filteredProperties" :key="property.id" class="hover:bg-slate-50/50 transition-colors group">
                  <td class="px-6 py-4">
                    <div class="flex items-center gap-4">
                      <img :src="property.image" :alt="property.title" class="w-16 h-12 rounded-lg object-cover shadow-sm group-hover:scale-105 transition-transform duration-300" />
                      <div>
                        <div class="font-semibold text-slate-800 text-sm">{{ property.title }}</div>
                        <div class="text-xs text-slate-500 mt-0.5 flex items-center gap-1">
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-3 h-3">
                            <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/>
                          </svg>
                          {{ property.location }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4">
                    <span class="inline-flex items-center px-2.5 py-1 rounded-md text-xs font-medium bg-slate-100 text-slate-600">
                      {{ property.typeText }}
                    </span>
                  </td>
                  <td class="px-6 py-4">
                    <div class="font-bold text-[#DAA520] text-sm">฿{{ formatPrice(property.price) }}</div>
                  </td>
                  <td class="px-6 py-4">
                    <span 
                      class="inline-flex items-center px-2.5 py-1 rounded-md text-xs font-semibold"
                      :class="{
                        'bg-emerald-500/10 text-emerald-600': property.status === 'active',
                        'bg-amber-500/10 text-amber-600': property.status === 'pending',
                        'bg-indigo-500/10 text-indigo-600': property.status === 'sold'
                      }"
                    >
                      <span class="w-1.5 h-1.5 rounded-full mr-1.5" :class="{
                        'bg-emerald-500': property.status === 'active',
                        'bg-amber-500': property.status === 'pending',
                        'bg-indigo-500': property.status === 'sold'
                      }"></span>
                      {{ property.statusText }}
                    </span>
                  </td>
                  <td class="px-6 py-4 text-sm text-slate-500">
                    {{ property.date }}
                  </td>
                  <td class="px-6 py-4 text-right">
                    <div class="flex items-center justify-end gap-2 opacity-0 group-hover:opacity-100 transition-opacity">
                      <button class="p-2 rounded-lg text-slate-400 hover:text-[#DAA520] hover:bg-amber-50 transition-colors" title="แก้ไข" @click="openEditModal(property)">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4">
                          <path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/>
                        </svg>
                      </button>
                      <button class="p-2 rounded-lg text-slate-400 hover:text-red-500 hover:bg-red-50 transition-colors" title="ลบ" @click="confirmDelete(property)">
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
          
          <!-- Pagination -->
          <div class="px-6 py-4 border-t border-slate-100 flex items-center justify-between">
            <div class="text-sm text-slate-500">
              แสดง <span class="font-medium text-slate-800">1-10</span> จาก <span class="font-medium text-slate-800">128</span> รายการ
            </div>
            <div class="flex gap-2">
              <button class="p-2 rounded-lg border border-slate-200 text-slate-500 hover:bg-slate-50 disabled:opacity-50 disabled:cursor-not-allowed" disabled>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><polyline points="15 18 9 12 15 6"/></svg>
              </button>
              <button class="px-3.5 py-2 rounded-lg bg-[#DAA520] text-white text-sm font-medium shadow-sm shadow-amber-500/20">1</button>
              <button class="px-3.5 py-2 rounded-lg border border-slate-200 text-slate-600 hover:bg-slate-50 text-sm font-medium transition-colors">2</button>
              <button class="px-3.5 py-2 rounded-lg border border-slate-200 text-slate-600 hover:bg-slate-50 text-sm font-medium transition-colors">3</button>
              <span class="px-2 py-2 text-slate-400">...</span>
              <button class="px-3.5 py-2 rounded-lg border border-slate-200 text-slate-600 hover:bg-slate-50 text-sm font-medium transition-colors">12</button>
              <button class="p-2 rounded-lg border border-slate-200 text-slate-500 hover:bg-slate-50 transition-colors">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><polyline points="9 18 15 12 9 6"/></svg>
              </button>
            </div>
          </div>
        </div>
      </main>
    </div>

    <!-- Add/Edit Modal -->
    <Teleport to="body">
      <div v-if="showModal" class="fixed inset-0 z-50 flex items-center justify-center p-4" @click.self="closeModal">
        <div class="absolute inset-0 bg-black/50 backdrop-blur-sm transition-opacity"></div>
        <div class="relative bg-white rounded-2xl shadow-2xl w-full max-w-4xl max-h-[90vh] flex flex-col animate-scale-in">
          
          <!-- Modal Header -->
          <div class="flex justify-between items-center p-6 border-b border-slate-100">
            <h2 class="text-xl font-bold text-slate-800">{{ isEditing ? 'แก้ไขทรัพย์สิน' : 'เพิ่มทรัพย์ใหม่' }}</h2>
            <button @click="closeModal" class="p-2 rounded-full hover:bg-slate-100 text-slate-400 hover:text-slate-600 transition-colors">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
            </button>
          </div>

          <!-- Modal Tabs -->
          <div class="flex border-b border-slate-100 px-6">
            <button 
              v-for="tab in modalTabs" 
              :key="tab.id"
              @click="activeModalTab = tab.id"
              class="px-4 py-3 text-sm font-medium border-b-2 transition-colors"
              :class="activeModalTab === tab.id ? 'border-[#DAA520] text-[#DAA520]' : 'border-transparent text-slate-500 hover:text-slate-700'"
            >
              {{ tab.label }}
            </button>
          </div>

          <!-- Modal Content (Scrollable) -->
          <div class="flex-1 overflow-y-auto p-6">
            
            <!-- General Tab -->
            <div v-if="activeModalTab === 'general'" class="space-y-6">
              <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="col-span-2 space-y-2">
                  <label class="text-sm font-medium text-slate-700">ชื่อทรัพย์สิน <span class="text-red-500">*</span></label>
                  <input type="text" v-model="form.title" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="ระบุชื่อทรัพย์สิน" />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">ชื่อโครงการ</label>
                  <input type="text" v-model="form.projectName" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">ประเภทประกาศ</label>
                  <select v-model="form.listingType" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all">
                    <option value="ขาย">ขาย</option>
                    <option value="เช่า">เช่า</option>
                  </select>
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">ประเภททรัพย์</label>
                  <select v-model="form.propertyType" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all">
                    <option value="คอนโด">คอนโด</option>
                    <option value="บ้านเดี่ยว">บ้านเดี่ยว</option>
                    <option value="ทาวน์โฮม">ทาวน์โฮม</option>
                    <option value="ที่ดิน">ที่ดิน</option>
                  </select>
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">ราคา (บาท) <span class="text-red-500">*</span></label>
                  <input type="number" v-model="form.price" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" />
                </div>
              </div>
            </div>

            <!-- Details Tab -->
            <div v-if="activeModalTab === 'details'" class="space-y-6">
              <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">ขนาด (ตร.ม.)</label>
                  <input type="number" v-model="form.size" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">ห้องนอน</label>
                  <input type="number" v-model="form.bedrooms" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">ห้องน้ำ</label>
                  <input type="number" v-model="form.bathrooms" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">ชั้น</label>
                  <input type="number" v-model="form.floor" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">อาคาร</label>
                  <input type="text" v-model="form.building" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">ทิศ</label>
                  <input type="text" v-model="form.direction" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" />
                </div>
              </div>

              <div class="space-y-2">
                <label class="text-sm font-medium text-slate-700">รายละเอียดเพิ่มเติม</label>
                <RichTextEditor v-model="form.description" />
              </div>

              <div class="space-y-2">
                <label class="text-sm font-medium text-slate-700">สิ่งอำนวยความสะดวก (คั่นด้วยจุลภาค)</label>
                <input type="text" v-model="form.facilities" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="สระว่ายน้ำ, ฟิตเนส, รปภ. 24 ชม." />
              </div>

              <div class="space-y-2">
                <label class="text-sm font-medium text-slate-700">สิ่งที่มีให้ในห้อง (คั่นด้วยจุลภาค)</label>
                <input type="text" v-model="form.amenities" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="เฟอร์นิเจอร์ครบ, เครื่องปรับอากาศ, ทีวี" />
              </div>
            </div>

            <!-- Media Tab -->
            <div v-if="activeModalTab === 'media'" class="space-y-6">
              <div class="space-y-2">
                <label class="text-sm font-medium text-slate-700">รูปภาพทรัพย์สิน</label>
                <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4">
                  <div v-for="(img, idx) in form.images" :key="idx" class="relative aspect-square rounded-xl overflow-hidden border border-slate-200 group">
                    <img :src="img" class="w-full h-full object-cover" />
                    <button @click="removeImage(idx)" class="absolute top-2 right-2 p-1.5 bg-red-500 text-white rounded-full opacity-0 group-hover:opacity-100 transition-opacity">
                      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
                    </button>
                  </div>
                  <label class="flex flex-col items-center justify-center aspect-square border-2 border-dashed border-slate-300 rounded-xl cursor-pointer hover:border-[#DAA520] hover:bg-amber-50/10 transition-all">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8 text-slate-400 mb-2"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/></svg>
                    <span class="text-xs text-slate-500">เพิ่มรูปภาพ</span>
                    <input type="file" class="hidden" multiple accept="image/*" @change="handleImageUpload" />
                  </label>
                </div>
              </div>
            </div>

            <!-- Location & Agent Tab -->
            <div v-if="activeModalTab === 'location'" class="space-y-6">
              <div class="space-y-2">
                <label class="text-sm font-medium text-slate-700">ที่ตั้ง / ทำเล</label>
                <input type="text" v-model="form.location" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" />
              </div>
              
              <div class="pt-6 border-t border-slate-100">
                <h3 class="text-lg font-bold text-slate-800 mb-4">ข้อมูลตัวแทน (Agent)</h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                  <div class="space-y-2">
                    <label class="text-sm font-medium text-slate-700">ชื่อตัวแทน</label>
                    <input type="text" v-model="form.agent.name" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" />
                  </div>
                  <div class="space-y-2">
                    <label class="text-sm font-medium text-slate-700">บริษัท / สังกัด</label>
                    <input type="text" v-model="form.agent.company" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" />
                  </div>
                  <div class="space-y-2">
                    <label class="text-sm font-medium text-slate-700">เบอร์โทรศัพท์</label>
                    <input type="text" v-model="form.agent.phone" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" />
                  </div>
                </div>
              </div>
            </div>

          </div>

          <!-- Modal Footer -->
          <div class="p-6 border-t border-slate-100 flex justify-end gap-3 bg-slate-50 rounded-b-2xl">
            <button @click="closeModal" class="px-6 py-2.5 rounded-xl border border-slate-200 bg-white text-slate-600 hover:bg-slate-50 font-medium transition-colors">ยกเลิก</button>
            <button @click="saveProperty" class="px-6 py-2.5 rounded-xl bg-[#DAA520] text-white hover:bg-[#b8860b] font-medium shadow-lg shadow-amber-500/20 transition-all">
              {{ isEditing ? 'บันทึกการแก้ไข' : 'เพิ่มทรัพย์' }}
            </button>
          </div>
        </div>
      </div>
    </Teleport>
  </div>
</template>

<script setup>
import { ref, computed, reactive } from 'vue'
import AdminSidebar from '~/components/admin/AdminSidebar.vue'
import AdminHeader from '~/components/admin/AdminHeader.vue'
import RichTextEditor from '~/components/admin/RichTextEditor.vue'

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

// Property Management State
const showModal = ref(false)
const isEditing = ref(false)
const activeModalTab = ref('general')
const searchQuery = ref('')
const filters = ref({
  type: '',
  status: '',
  price: ''
})

const modalTabs = [
  { id: 'general', label: 'ข้อมูลทั่วไป' },
  { id: 'details', label: 'รายละเอียด' },
  { id: 'media', label: 'รูปภาพ' },
  { id: 'location', label: 'ทำเล & ตัวแทน' }
]

const defaultForm = {
  id: null,
  title: '',
  projectName: '',
  listingType: 'ขาย',
  propertyType: 'คอนโด',
  price: '',
  size: '',
  bedrooms: '',
  bathrooms: '',
  floor: '',
  building: '',
  direction: '',
  description: '',
  facilities: '',
  amenities: '',
  location: '',
  images: [],
  agent: {
    name: 'Mr.Best RealHome',
    company: 'DD Expert Condo',
    phone: ''
  },
  status: 'active'
}

const form = ref({ ...defaultForm })

const stats = [
  { label: 'ทรัพย์ทั้งหมด', value: '1,248', trend: 12 },
  { label: 'Active', value: '856', trend: 5 },
  { label: 'Pending', value: '45', trend: -2 },
  { label: 'Sold (เดือนนี้)', value: '28', trend: 8 }
]

const properties = ref([
  { 
    id: 1, 
    title: 'Ideo Q สุขุมวิท 36', 
    location: 'สุขุมวิท, กรุงเทพฯ', 
    price: 4990000, 
    type: 'condo',
    typeText: 'คอนโด',
    image: 'https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=100&h=80&fit=crop', 
    status: 'active', 
    statusText: 'Active',
    date: '28 ธ.ค. 2024'
  },
  // ... other mock data
])

const filteredProperties = computed(() => {
  return properties.value.filter(p => {
    if (searchQuery.value && !p.title.toLowerCase().includes(searchQuery.value.toLowerCase())) return false
    if (filters.value.type && p.type !== filters.value.type) return false
    if (filters.value.status && p.status !== filters.value.status) return false
    return true
  })
})

const formatPrice = (price) => new Intl.NumberFormat('th-TH').format(price)

const openAddModal = () => {
  isEditing.value = false
  form.value = JSON.parse(JSON.stringify(defaultForm))
  activeModalTab.value = 'general'
  showModal.value = true
}

const openEditModal = (property) => {
  isEditing.value = true
  // In a real app, you'd fetch full details here. For now, we mock populate.
  form.value = {
    ...JSON.parse(JSON.stringify(defaultForm)),
    ...property,
    // Mocking missing fields for demo
    projectName: property.title,
    listingType: 'ขาย',
    propertyType: property.typeText,
    size: 35,
    bedrooms: 1,
    bathrooms: 1,
    description: 'รายละเอียดทรัพย์...',
    images: [property.image]
  }
  activeModalTab.value = 'general'
  showModal.value = true
}

const closeModal = () => {
  showModal.value = false
}

const handleImageUpload = (event) => {
  const files = event.target.files
  if (files) {
    for (let i = 0; i < files.length; i++) {
      form.value.images.push(URL.createObjectURL(files[i]))
    }
  }
}

const removeImage = (index) => {
  form.value.images.splice(index, 1)
}

const saveProperty = () => {
  if (!form.value.title || !form.value.price) {
    alert('กรุณากรอกข้อมูลจำเป็นให้ครบถ้วน')
    return
  }
  
  // Save logic here (API call)
  console.log('Saving property:', form.value)
  alert('บันทึกข้อมูลเรียบร้อยแล้ว')
  closeModal()
}

const exportProperties = () => {
  alert('Exporting properties...')
}

const confirmDelete = (property) => {
  if (confirm(`ต้องการลบ ${property.title} ใช่หรือไม่?`)) {
    alert('Deleted')
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

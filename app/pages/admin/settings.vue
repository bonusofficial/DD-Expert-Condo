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
          <h1 class="text-2xl font-bold text-slate-800">System Settings</h1>
          <p class="text-slate-500 mt-1">จัดการการตั้งค่าระบบและ SEO</p>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-[240px_1fr] gap-8">
          <!-- Sidebar Navigation -->
          <nav class="space-y-1">
            <button
              v-for="tab in tabs"
              :key="tab.id"
              @click="activeTab = tab.id"
              class="w-full flex items-center gap-3 px-4 py-3 rounded-xl text-sm font-medium transition-all duration-200"
              :class="activeTab === tab.id ? 'bg-white text-[#DAA520] shadow-sm ring-1 ring-slate-100' : 'text-slate-500 hover:bg-white hover:text-slate-700'"
            >
              <span class="w-8 h-8 rounded-lg flex items-center justify-center transition-colors" :class="activeTab === tab.id ? 'bg-amber-50' : 'bg-slate-50 group-hover:bg-slate-100'">
                <svg v-if="tab.id === 'general'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg>
                <svg v-if="tab.id === 'property_types'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg>
                <svg v-if="tab.id === 'seo'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><circle cx="12" cy="12" r="10"/><line x1="2" y1="12" x2="22" y2="12"/><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/></svg>
                <svg v-if="tab.id === 'social'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><path d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"/></svg>
                <svg v-if="tab.id === 'menu'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="18" x2="21" y2="18"/></svg>
                <svg v-if="tab.id === 'advanced'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><polyline points="16 18 22 12 16 6"/><polyline points="8 6 2 12 8 18"/></svg>
              </span>
              {{ tab.name }}
            </button>
          </nav>

          <!-- Content Area -->
          <div class="space-y-6">
            <!-- General Settings -->
            <div v-if="activeTab === 'general'" class="space-y-6">
              <!-- Localization Defaults -->
              <div class="bg-white rounded-2xl shadow-sm border border-slate-100 p-6 animate-fade-in">
                <h2 class="text-lg font-bold text-slate-800 mb-6 pb-4 border-b border-slate-100">Localization Defaults</h2>
                <div class="space-y-6 max-w-2xl">
                  <div class="space-y-2">
                    <label class="text-sm font-medium text-slate-700">ภาษาเริ่มต้น (Default Language)</label>
                    <select v-model="localization.language" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all appearance-none cursor-pointer">
                      <option v-for="country in countries" :key="country.code" :value="country.code">
                        {{ country.flag }} {{ country.name }} ({{ country.language }})
                      </option>
                    </select>
                  </div>
                  
                  <div class="space-y-2">
                    <label class="text-sm font-medium text-slate-700">สกุลเงินเริ่มต้น (Default Currency)</label>
                    <select v-model="localization.currency" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all appearance-none cursor-pointer">
                      <option v-for="country in countries" :key="country.currencyCode" :value="country.currencyCode">
                        {{ country.currencySymbol }} {{ country.currencyCode }} ({{ country.currencyName }})
                      </option>
                    </select>
                  </div>
                </div>
              </div>

              <!-- Supported Countries Management -->
              <div class="bg-white rounded-2xl shadow-sm border border-slate-100 p-6 animate-fade-in">
                <div class="flex justify-between items-center mb-6 pb-4 border-b border-slate-100">
                  <div>
                    <h2 class="text-lg font-bold text-slate-800">Supported Countries</h2>
                    <p class="text-sm text-slate-500">จัดการประเทศและภาษาที่รองรับในระบบ</p>
                  </div>
                  <button @click="openAddCountryModal" class="flex items-center gap-2 px-4 py-2 rounded-lg bg-[#DAA520] text-white text-sm font-medium hover:bg-[#b8860b] transition-colors shadow-sm shadow-amber-500/20">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
                    เพิ่มประเทศ
                  </button>
                </div>

                <div class="overflow-hidden rounded-xl border border-slate-200">
                  <table class="w-full text-left border-collapse">
                    <thead class="bg-slate-50 text-xs uppercase text-slate-500 font-semibold">
                      <tr>
                        <th class="px-4 py-3">Country</th>
                        <th class="px-4 py-3">Language</th>
                        <th class="px-4 py-3">Currency</th>
                        <th class="px-4 py-3 text-right">Actions</th>
                      </tr>
                    </thead>
                    <tbody class="divide-y divide-slate-100">
                      <tr v-for="(country, index) in countries" :key="country.code" class="hover:bg-slate-50/50">
                        <td class="px-4 py-3">
                          <div class="flex items-center gap-3">
                            <span class="text-xl">{{ country.flag }}</span>
                            <div>
                              <div class="font-medium text-slate-800">{{ country.name }}</div>
                              <div class="text-xs text-slate-500">{{ country.code.toUpperCase() }}</div>
                            </div>
                          </div>
                        </td>
                        <td class="px-4 py-3 text-sm text-slate-600">{{ country.language }}</td>
                        <td class="px-4 py-3 text-sm text-slate-600">
                          <span class="font-mono bg-slate-100 px-1.5 py-0.5 rounded text-slate-700">{{ country.currencySymbol }}</span> {{ country.currencyCode }}
                        </td>
                        <td class="px-4 py-3 text-right">
                          <button @click="removeCountry(index)" class="p-1.5 text-slate-400 hover:text-red-500 hover:bg-red-50 rounded-lg transition-colors" title="Remove">
                            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
                          </button>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>

            <!-- Property Types Settings -->
            <div v-if="activeTab === 'property_types'" class="space-y-6">
              <div class="bg-white rounded-2xl shadow-sm border border-slate-100 p-6 animate-fade-in">
                <div class="flex justify-between items-center mb-6 pb-4 border-b border-slate-100">
                  <div>
                    <h2 class="text-lg font-bold text-slate-800">Property Types</h2>
                    <p class="text-sm text-slate-500">จัดการประเภทอสังหาริมทรัพย์ ไอคอน และรูปภาพแบนเนอร์</p>
                  </div>
                  <button @click="openPropertyTypeModal" class="flex items-center gap-2 px-4 py-2 rounded-lg bg-[#DAA520] text-white text-sm font-medium hover:bg-[#b8860b] transition-colors shadow-sm shadow-amber-500/20">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
                    เพิ่มประเภท
                  </button>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                  <div v-for="(type, index) in propertyTypes" :key="type.id" class="group relative bg-slate-50 rounded-xl border border-slate-200 overflow-hidden hover:shadow-md transition-all">
                    <!-- Banner -->
                    <div class="h-32 bg-slate-200 relative">
                      <img :src="type.banner" class="w-full h-full object-cover" />
                      <div class="absolute inset-0 bg-black/20 group-hover:bg-black/10 transition-colors"></div>
                    </div>
                    <!-- Icon & Info -->
                    <div class="p-4 relative">
                      <div class="absolute -top-8 left-4 w-16 h-16 bg-white rounded-xl shadow-sm border border-slate-100 flex items-center justify-center text-[#DAA520]" v-html="type.icon"></div>
                      <div class="mt-8 flex justify-between items-start">
                        <div>
                          <h3 class="font-bold text-slate-800">{{ type.name }}</h3>
                          <p class="text-xs text-slate-500">{{ type.count || 0 }} Properties</p>
                        </div>
                        <div class="flex gap-1">
                          <button @click="editPropertyType(type)" class="p-1.5 text-slate-400 hover:text-[#DAA520] hover:bg-amber-50 rounded-lg transition-colors">
                            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/></svg>
                          </button>
                          <button @click="removePropertyType(index)" class="p-1.5 text-slate-400 hover:text-red-500 hover:bg-red-50 rounded-lg transition-colors">
                            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <!-- Menu Management Settings -->
            <div v-if="activeTab === 'menu'" class="space-y-6">
              <div class="bg-white rounded-2xl shadow-sm border border-slate-100 p-6 animate-fade-in">
                <div class="flex justify-between items-center mb-6 pb-4 border-b border-slate-100">
                  <div>
                    <h2 class="text-lg font-bold text-slate-800">Navigation Menu</h2>
                    <p class="text-sm text-slate-500">จัดการเมนูนำทางบนเว็บไซต์ ลากเพื่อเรียงลำดับ</p>
                  </div>
                  <button @click="openMenuModal" class="flex items-center gap-2 px-4 py-2 rounded-lg bg-[#DAA520] text-white text-sm font-medium hover:bg-[#b8860b] transition-colors shadow-sm shadow-amber-500/20">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
                    เพิ่มเมนู
                  </button>
                </div>

                <!-- Menu Items List -->
                <div class="space-y-3">
                  <div 
                    v-for="(item, index) in menuItems" 
                    :key="item.id" 
                    class="group flex items-center gap-4 p-4 bg-slate-50 rounded-xl border border-slate-200 hover:border-[#DAA520]/50 hover:shadow-sm transition-all"
                  >
                    <!-- Drag Handle -->
                    <div class="cursor-grab active:cursor-grabbing text-slate-400 hover:text-slate-600">
                      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5">
                        <circle cx="9" cy="5" r="1"/><circle cx="9" cy="12" r="1"/><circle cx="9" cy="19" r="1"/>
                        <circle cx="15" cy="5" r="1"/><circle cx="15" cy="12" r="1"/><circle cx="15" cy="19" r="1"/>
                      </svg>
                    </div>

                    <!-- Icon Preview -->
                    <div class="w-10 h-10 rounded-lg bg-white border border-slate-200 flex items-center justify-center text-[#DAA520] flex-shrink-0" v-html="item.icon"></div>

                    <!-- Menu Info -->
                    <div class="flex-1 min-w-0">
                      <h3 class="font-medium text-slate-800 truncate">{{ item.name }}</h3>
                      <p class="text-xs text-slate-500 truncate">{{ item.url }}</p>
                    </div>

                    <!-- Status Badge -->
                    <span 
                      class="px-2.5 py-1 rounded-full text-xs font-medium"
                      :class="item.isActive ? 'bg-emerald-100 text-emerald-700' : 'bg-slate-200 text-slate-600'"
                    >
                      {{ item.isActive ? 'แสดง' : 'ซ่อน' }}
                    </span>

                    <!-- Actions -->
                    <div class="flex gap-1 opacity-0 group-hover:opacity-100 transition-opacity">
                      <button 
                        @click="toggleMenuStatus(index)" 
                        class="p-2 rounded-lg hover:bg-white text-slate-400 hover:text-slate-600 transition-colors"
                        :title="item.isActive ? 'ซ่อนเมนู' : 'แสดงเมนู'"
                      >
                        <svg v-if="item.isActive" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/><circle cx="12" cy="12" r="3"/></svg>
                        <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"/><line x1="1" y1="1" x2="23" y2="23"/></svg>
                      </button>
                      <button @click="editMenuItem(item)" class="p-2 rounded-lg hover:bg-white text-slate-400 hover:text-[#DAA520] transition-colors" title="แก้ไข">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/></svg>
                      </button>
                      <button @click="removeMenuItem(index)" class="p-2 rounded-lg hover:bg-white text-slate-400 hover:text-red-500 transition-colors" title="ลบ">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/></svg>
                      </button>
                    </div>

                    <!-- Order Badge -->
                    <div class="w-8 h-8 rounded-full bg-slate-200 flex items-center justify-center text-xs font-bold text-slate-600">
                      {{ index + 1 }}
                    </div>
                  </div>

                  <!-- Empty State -->
                  <div v-if="menuItems.length === 0" class="text-center py-12">
                    <div class="w-16 h-16 mx-auto mb-4 rounded-full bg-slate-100 flex items-center justify-center">
                      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8 text-slate-400"><line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="18" x2="21" y2="18"/></svg>
                    </div>
                    <h3 class="font-medium text-slate-600 mb-1">ยังไม่มีเมนู</h3>
                    <p class="text-sm text-slate-500">คลิก "เพิ่มเมนู" เพื่อเริ่มสร้างเมนูนำทาง</p>
                  </div>
                </div>

                <!-- Move Buttons (for mobile) -->
                <div v-if="menuItems.length > 1" class="mt-6 pt-4 border-t border-slate-100">
                  <p class="text-xs text-slate-500 mb-3">ใช้ปุ่มด้านบนเพื่อลากเรียงลำดับเมนู หรือใช้ปุ่มด้านล่างสำหรับ Mobile</p>
                  <div class="flex flex-wrap gap-2">
                    <button 
                      v-for="(item, index) in menuItems" 
                      :key="item.id"
                      class="flex items-center gap-2 px-3 py-1.5 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-600 text-sm transition-colors"
                    >
                      <span class="w-5 h-5 rounded bg-slate-300 flex items-center justify-center text-xs font-medium">{{ index + 1 }}</span>
                      <span class="truncate max-w-[100px]">{{ item.name }}</span>
                      <div class="flex gap-0.5">
                        <button @click="moveMenuUp(index)" :disabled="index === 0" class="p-0.5 hover:text-[#DAA520] disabled:opacity-30 disabled:cursor-not-allowed">
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-3.5 h-3.5"><polyline points="18 15 12 9 6 15"/></svg>
                        </button>
                        <button @click="moveMenuDown(index)" :disabled="index === menuItems.length - 1" class="p-0.5 hover:text-[#DAA520] disabled:opacity-30 disabled:cursor-not-allowed">
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-3.5 h-3.5"><polyline points="6 9 12 15 18 9"/></svg>
                        </button>
                      </div>
                    </button>
                  </div>
                </div>
              </div>
            </div>

            <!-- Global SEO Settings -->
            <div v-if="activeTab === 'seo'" class="bg-white rounded-2xl shadow-sm border border-slate-100 p-6 animate-fade-in">
              <h2 class="text-lg font-bold text-slate-800 mb-6 pb-4 border-b border-slate-100">Global SEO Settings</h2>
              <div class="space-y-6 max-w-3xl">
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">Default Page Title</label>
                  <div class="flex gap-2">
                    <input type="text" v-model="seo.defaultTitle" class="flex-1 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="e.g. DDexpert Condo - คอนโดมือสองคุณภาพ" />
                  </div>
                  <p class="text-xs text-slate-500">ชื่อที่จะแสดงบนแท็บเบราว์เซอร์เมื่อไม่มีการระบุชื่อเฉพาะ</p>
                </div>

                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">Meta Description</label>
                  <textarea v-model="seo.metaDescription" rows="3" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="คำอธิบายเว็บไซต์โดยย่อ..."></textarea>
                  <div class="flex justify-between text-xs text-slate-500">
                    <span>ควรมีความยาวประมาณ 150-160 ตัวอักษร</span>
                    <span :class="seo.metaDescription.length > 160 ? 'text-red-500' : 'text-emerald-600'">{{ seo.metaDescription.length }}/160</span>
                  </div>
                </div>

                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">Meta Keywords</label>
                  <input type="text" v-model="seo.keywords" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="คอนโด, ซื้อคอนโด, ขายคอนโด, คอนโดมือสอง" />
                  <p class="text-xs text-slate-500">คั่นด้วยเครื่องหมายจุลภาค (,)</p>
                </div>
              </div>
            </div>

            <!-- Social Media SEO -->
            <div v-if="activeTab === 'social'" class="bg-white rounded-2xl shadow-sm border border-slate-100 p-6 animate-fade-in">
              <h2 class="text-lg font-bold text-slate-800 mb-6 pb-4 border-b border-slate-100">Social Media (Open Graph)</h2>
              <div class="space-y-6 max-w-3xl">
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">Default OG Image</label>
                  <div class="relative group">
                    <div v-if="seo.ogImage" class="relative w-full aspect-[1200/630] rounded-xl overflow-hidden border border-slate-200 bg-slate-100 max-w-md">
                      <img :src="seo.ogImage" class="w-full h-full object-cover" />
                      <button @click="removeOgImage" class="absolute top-2 right-2 p-1.5 bg-red-500 text-white rounded-full hover:bg-red-600 transition-colors shadow-sm">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
                      </button>
                    </div>
                    <label v-else class="flex flex-col items-center justify-center w-full max-w-md aspect-[1200/630] border-2 border-slate-300 border-dashed rounded-xl cursor-pointer bg-slate-50 hover:bg-slate-100 hover:border-[#DAA520] transition-all">
                      <div class="flex flex-col items-center justify-center pt-5 pb-6">
                        <svg class="w-10 h-10 mb-3 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"></path></svg>
                        <p class="mb-1 text-sm text-slate-500"><span class="font-semibold text-[#DAA520]">คลิกเพื่ออัปโหลด</span></p>
                        <p class="text-xs text-slate-400">ขนาดแนะนำ 1200x630px</p>
                      </div>
                      <input type="file" class="hidden" accept="image/*" @change="handleOgImageUpload" />
                    </label>
                  </div>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                  <div class="space-y-2">
                    <label class="text-sm font-medium text-slate-700">Facebook App ID</label>
                    <input type="text" v-model="seo.fbAppId" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="1234567890" />
                  </div>
                  <div class="space-y-2">
                    <label class="text-sm font-medium text-slate-700">Twitter Handle</label>
                    <input type="text" v-model="seo.twitterHandle" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="@ddexpert" />
                  </div>
                </div>
              </div>
            </div>

            <!-- Advanced SEO -->
            <div v-if="activeTab === 'advanced'" class="bg-white rounded-2xl shadow-sm border border-slate-100 p-6 animate-fade-in">
              <h2 class="text-lg font-bold text-slate-800 mb-6 pb-4 border-b border-slate-100">Robots & Sitemap</h2>
              <div class="space-y-6 max-w-3xl">
                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">Robots.txt Content</label>
                  <textarea v-model="seo.robotsTxt" rows="6" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all font-mono text-sm" placeholder="User-agent: *&#10;Allow: /"></textarea>
                </div>

                <div class="space-y-2">
                  <label class="text-sm font-medium text-slate-700">Sitemap URL</label>
                  <div class="flex gap-2">
                    <input type="text" value="https://ddexpert-condo.com/sitemap.xml" readonly class="flex-1 px-4 py-2.5 bg-slate-100 border border-slate-200 rounded-xl text-slate-500 outline-none" />
                    <button class="px-4 py-2.5 rounded-xl border border-slate-200 bg-white text-slate-600 hover:bg-slate-50 font-medium transition-colors">
                      Regenerate
                    </button>
                  </div>
                </div>
              </div>
            </div>

            <!-- Save Button -->
            <div class="flex justify-end pt-4">
              <button class="flex items-center gap-2 px-6 py-3 rounded-xl bg-[#DAA520] text-white hover:bg-[#b8860b] font-medium shadow-lg shadow-amber-500/20 transition-all hover:-translate-y-0.5" @click="saveSettings">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"/><polyline points="17 21 17 13 7 13 7 21"/><polyline points="7 3 7 8 15 8"/></svg>
                บันทึกการตั้งค่า
              </button>
            </div>
          </div>
        </div>
      </main>
    </div>

    <!-- Add Country Modal -->
    <Teleport to="body">
      <div v-if="showCountryModal" class="fixed inset-0 z-50 flex items-center justify-center p-4" @click.self="closeCountryModal">
        <div class="absolute inset-0 bg-black/50 backdrop-blur-sm transition-opacity"></div>
        <div class="relative bg-white rounded-2xl shadow-2xl w-full max-w-md animate-scale-in">
          <div class="flex justify-between items-center p-6 border-b border-slate-100">
            <h2 class="text-xl font-bold text-slate-800">เพิ่มประเทศใหม่</h2>
            <button @click="closeCountryModal" class="p-2 rounded-full hover:bg-slate-100 text-slate-400 hover:text-slate-600 transition-colors">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
            </button>
          </div>
          <div class="p-6 space-y-4">
            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">ชื่อประเทศ (Country Name)</label>
              <input type="text" v-model="newCountry.name" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="e.g. Japan" />
            </div>
            <div class="grid grid-cols-2 gap-4">
              <div class="space-y-2">
                <label class="text-sm font-medium text-slate-700">รหัสประเทศ (Code)</label>
                <input type="text" v-model="newCountry.code" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="e.g. jp" />
              </div>
              <div class="space-y-2">
                <label class="text-sm font-medium text-slate-700">ธง (Emoji)</label>
                <input type="text" v-model="newCountry.flag" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="e.g. 🇯🇵" />
              </div>
            </div>
            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">ภาษา (Language Name)</label>
              <input type="text" v-model="newCountry.language" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="e.g. Japanese" />
            </div>
            <div class="grid grid-cols-2 gap-4">
              <div class="space-y-2">
                <label class="text-sm font-medium text-slate-700">รหัสสกุลเงิน</label>
                <input type="text" v-model="newCountry.currencyCode" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="e.g. JPY" />
              </div>
              <div class="space-y-2">
                <label class="text-sm font-medium text-slate-700">สัญลักษณ์เงิน</label>
                <input type="text" v-model="newCountry.currencySymbol" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="e.g. ¥" />
              </div>
            </div>
            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">ชื่อสกุลเงิน</label>
              <input type="text" v-model="newCountry.currencyName" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="e.g. Japanese Yen" />
            </div>
          </div>
          <div class="p-6 border-t border-slate-100 flex justify-end gap-3 bg-slate-50 rounded-b-2xl">
            <button @click="closeCountryModal" class="px-6 py-2.5 rounded-xl border border-slate-200 bg-white text-slate-600 hover:bg-slate-50 font-medium transition-colors">ยกเลิก</button>
            <button @click="addCountry" class="px-6 py-2.5 rounded-xl bg-[#DAA520] text-white hover:bg-[#b8860b] font-medium shadow-lg shadow-amber-500/20 transition-all">เพิ่มประเทศ</button>
          </div>
        </div>
      </div>
    </Teleport>

    <!-- Add Property Type Modal -->
    <Teleport to="body">
      <div v-if="showPropertyTypeModal" class="fixed inset-0 z-50 flex items-center justify-center p-4" @click.self="closePropertyTypeModal">
        <div class="absolute inset-0 bg-black/50 backdrop-blur-sm transition-opacity"></div>
        <div class="relative bg-white rounded-2xl shadow-2xl w-full max-w-lg animate-scale-in">
          <div class="flex justify-between items-center p-6 border-b border-slate-100">
            <h2 class="text-xl font-bold text-slate-800">{{ isEditingType ? 'แก้ไขประเภท' : 'เพิ่มประเภทใหม่' }}</h2>
            <button @click="closePropertyTypeModal" class="p-2 rounded-full hover:bg-slate-100 text-slate-400 hover:text-slate-600 transition-colors">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
            </button>
          </div>
          <div class="p-6 space-y-6">
            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">ชื่อประเภท (Type Name)</label>
              <input type="text" v-model="propertyTypeForm.name" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="e.g. Condo" />
            </div>

            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">ไอคอน (Select Icon)</label>
              <div class="grid grid-cols-4 gap-2 max-h-60 overflow-y-auto pr-2 custom-scrollbar">
                <button 
                  v-for="icon in availableIcons" 
                  :key="icon.name"
                  @click="selectIcon(icon.svg)"
                  class="p-3 rounded-xl border flex flex-col items-center justify-center gap-2 transition-all"
                  :class="propertyTypeForm.icon === icon.svg ? 'border-[#DAA520] bg-amber-50 text-[#DAA520]' : 'border-slate-200 hover:border-slate-300 text-slate-500'"
                >
                  <div class="w-6 h-6" v-html="icon.svg"></div>
                  <span class="text-[10px]">{{ icon.name }}</span>
                </button>
              </div>
            </div>

            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">รูปภาพแบนเนอร์ (Banner Image)</label>
              <div class="relative group w-full h-40 rounded-xl overflow-hidden border-2 border-dashed border-slate-300 bg-slate-50 hover:bg-slate-100 transition-all">
                <img v-if="propertyTypeForm.banner" :src="propertyTypeForm.banner" class="w-full h-full object-cover" />
                <div v-else class="absolute inset-0 flex flex-col items-center justify-center text-slate-400">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8 mb-2"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/></svg>
                  <span class="text-xs">Upload Banner</span>
                </div>
                <input type="file" class="absolute inset-0 opacity-0 cursor-pointer" accept="image/*" @change="handleBannerUpload" />
              </div>
            </div>
          </div>
          <div class="p-6 border-t border-slate-100 flex justify-end gap-3 bg-slate-50 rounded-b-2xl">
            <button @click="closePropertyTypeModal" class="px-6 py-2.5 rounded-xl border border-slate-200 bg-white text-slate-600 hover:bg-slate-50 font-medium transition-colors">ยกเลิก</button>
            <button @click="savePropertyType" class="px-6 py-2.5 rounded-xl bg-[#DAA520] text-white hover:bg-[#b8860b] font-medium shadow-lg shadow-amber-500/20 transition-all">บันทึก</button>
          </div>
        </div>
      </div>
    </Teleport>

    <!-- Add/Edit Menu Item Modal -->
    <Teleport to="body">
      <div v-if="showMenuModal" class="fixed inset-0 z-50 flex items-center justify-center p-4" @click.self="closeMenuModal">
        <div class="absolute inset-0 bg-black/50 backdrop-blur-sm transition-opacity"></div>
        <div class="relative bg-white rounded-2xl shadow-2xl w-full max-w-lg animate-scale-in max-h-[90vh] overflow-hidden flex flex-col">
          <div class="flex justify-between items-center p-6 border-b border-slate-100 flex-shrink-0">
            <h2 class="text-xl font-bold text-slate-800">{{ isEditingMenu ? 'แก้ไขเมนู' : 'เพิ่มเมนูใหม่' }}</h2>
            <button @click="closeMenuModal" class="p-2 rounded-full hover:bg-slate-100 text-slate-400 hover:text-slate-600 transition-colors">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
            </button>
          </div>
          <div class="p-6 space-y-6 overflow-y-auto flex-1 custom-scrollbar">
            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">ชื่อเมนู (Menu Name) *</label>
              <input type="text" v-model="menuForm.name" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all" placeholder="e.g. หน้าแรก, อสังหาริมทรัพย์" />
            </div>

            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">URL / ลิงก์ *</label>
              <input type="text" v-model="menuForm.url" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all font-mono text-sm" placeholder="e.g. /, /properties, /about" />
              <p class="text-xs text-slate-500">ใส่ / สำหรับหน้าแรก หรือ URL ภายในเช่น /properties</p>
            </div>

            <div class="space-y-2">
              <label class="text-sm font-medium text-slate-700">ไอคอน (Select Icon)</label>
              <div class="grid grid-cols-6 gap-2 max-h-48 overflow-y-auto pr-2 custom-scrollbar">
                <button 
                  v-for="icon in menuIcons" 
                  :key="icon.name"
                  @click="selectMenuIcon(icon.svg)"
                  class="p-2.5 rounded-lg border flex flex-col items-center justify-center gap-1 transition-all"
                  :class="menuForm.icon === icon.svg ? 'border-[#DAA520] bg-amber-50 text-[#DAA520]' : 'border-slate-200 hover:border-slate-300 text-slate-500'"
                >
                  <div class="w-5 h-5" v-html="icon.svg"></div>
                  <span class="text-[8px] truncate w-full text-center">{{ icon.name }}</span>
                </button>
              </div>
            </div>

            <div class="flex items-center justify-between p-4 bg-slate-50 rounded-xl">
              <div>
                <label class="text-sm font-medium text-slate-700">แสดงเมนู</label>
                <p class="text-xs text-slate-500">เปิด/ปิดการแสดงผลเมนูนี้</p>
              </div>
              <button 
                @click="menuForm.isActive = !menuForm.isActive"
                class="relative w-12 h-6 rounded-full transition-colors"
                :class="menuForm.isActive ? 'bg-[#DAA520]' : 'bg-slate-300'"
              >
                <span 
                  class="absolute top-1 w-4 h-4 bg-white rounded-full shadow transition-transform"
                  :class="menuForm.isActive ? 'left-7' : 'left-1'"
                ></span>
              </button>
            </div>
          </div>
          <div class="p-6 border-t border-slate-100 flex justify-end gap-3 bg-slate-50 rounded-b-2xl flex-shrink-0">
            <button @click="closeMenuModal" class="px-6 py-2.5 rounded-xl border border-slate-200 bg-white text-slate-600 hover:bg-slate-50 font-medium transition-colors">ยกเลิก</button>
            <button @click="saveMenuItem" class="px-6 py-2.5 rounded-xl bg-[#DAA520] text-white hover:bg-[#b8860b] font-medium shadow-lg shadow-amber-500/20 transition-all">บันทึก</button>
          </div>
        </div>
      </div>
    </Teleport>
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

const activeTab = ref('general')

const tabs = [
  { id: 'general', name: 'General' },
  { id: 'property_types', name: 'Property Types' },
  { id: 'menu', name: 'Menu' },
  { id: 'seo', name: 'SEO' },
  { id: 'social', name: 'Social Media' },
  { id: 'advanced', name: 'Advanced' }
]

const localization = reactive({
  language: 'th',
  currency: 'THB'
})

// Property Types Management
const showPropertyTypeModal = ref(false)
const isEditingType = ref(false)
const propertyTypes = ref([
  { 
    id: 1, 
    name: 'Condo', 
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><rect x="4" y="2" width="16" height="20" rx="2" ry="2"></rect><line x1="9" y1="22" x2="9" y2="22.01"></line><line x1="15" y1="22" x2="15" y2="22.01"></line><line x1="12" y1="22" x2="12" y2="22.01"></line><rect x="8" y="6" width="8" height="12"></rect></svg>',
    banner: 'https://images.unsplash.com/photo-1545324418-cc1a3fa10c00?w=500&h=300&fit=crop',
    count: 156
  },
  { 
    id: 2, 
    name: 'House', 
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg>',
    banner: 'https://images.unsplash.com/photo-1518780664697-55e3ad937233?w=500&h=300&fit=crop',
    count: 84
  },
  { 
    id: 3, 
    name: 'Land', 
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M21.21 15.89A10 10 0 1 1 8 2.83"></path><path d="M22 12A10 10 0 0 0 12 2v10z"></path></svg>',
    banner: 'https://images.unsplash.com/photo-1500382017468-9049fed747ef?w=500&h=300&fit=crop',
    count: 42
  }
])

const availableIcons = [
  // Buildings & Structures
  { name: 'Building', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><rect x="4" y="2" width="16" height="20" rx="2" ry="2"></rect><line x1="9" y1="6" x2="9" y2="6.01"></line><line x1="15" y1="6" x2="15" y2="6.01"></line><line x1="9" y1="10" x2="9" y2="10.01"></line><line x1="15" y1="10" x2="15" y2="10.01"></line><line x1="9" y1="14" x2="9" y2="14.01"></line><line x1="15" y1="14" x2="15" y2="14.01"></line><path d="M9 18h6v4H9z"></path></svg>' },
  { name: 'Home', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg>' },
  { name: 'Land', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M21.21 15.89A10 10 0 1 1 8 2.83"></path><path d="M22 12A10 10 0 0 0 12 2v10z"></path></svg>' },
  { name: 'Townhome', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M3 21h18M5 21V7l8-4 8 4v14M8 21V12a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v9"></path></svg>' },
  { name: 'Office', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><rect x="4" y="2" width="16" height="20" rx="2"></rect><path d="M9 22v-4h6v4"></path><path d="M8 6h.01M16 6h.01M8 10h.01M16 10h.01M8 14h.01M16 14h.01M12 6h.01M12 10h.01M12 14h.01"></path></svg>' },
  { name: 'Factory', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2 20a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2V8l-7 5V8l-7 5V4a2 2 0 0 0-2-2H4a2 2 0 0 0-2 2Z"></path><path d="M17 18h1M12 18h1M7 18h1"></path></svg>' },
  { name: 'Apartment', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><rect x="4" y="2" width="16" height="20" rx="2"></rect><path d="M9 22v-4h6v4M8 6h2M14 6h2M8 10h2M14 10h2M8 14h2M14 14h2"></path></svg>' },
  { name: 'Villa', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2 22h20"></path><path d="M3 22V11l9-9 9 9v11"></path><path d="M9 22v-6a3 3 0 0 1 6 0v6"></path><circle cx="12" cy="9" r="2"></circle></svg>' },
  { name: 'Condo', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><rect x="2" y="4" width="20" height="18" rx="2"></rect><path d="M2 8h20M6 8v14M10 8v14M14 8v14M18 8v14"></path></svg>' },
  { name: 'Duplex', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2 22h20"></path><path d="M3 22V9l9-7 9 7v13"></path><path d="M12 22V2"></path><path d="M7 14h2v3H7zM15 14h2v3h-2z"></path></svg>' },
  { name: 'Shop', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M3 9l9-7 9 7"></path><path d="M21 9v10a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V9"></path><path d="M9 21V12h6v9"></path></svg>' },
  { name: 'Warehouse', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M22 8.35V20a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V8.35A2 2 0 0 1 3.26 6.5l8-3.2a2 2 0 0 1 1.48 0l8 3.2A2 2 0 0 1 22 8.35Z"></path><path d="M6 18h12M6 14h12M12 18v4"></path></svg>' },
  { name: 'Hotel', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M18 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V4a2 2 0 0 0-2-2Z"></path><path d="M9 22v-4h6v4M8 6h.01M16 6h.01M12 6h.01M8 10h.01M16 10h.01M12 10h.01M8 14h.01M16 14h.01M12 14h.01"></path></svg>' },
  { name: 'Resort', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M12 22v-9"></path><path d="M12 13a4 4 0 0 0-4-4H5a1 1 0 0 0-1 1v12"></path><path d="M12 13a4 4 0 0 1 4-4h3a1 1 0 0 1 1 1v12"></path><circle cx="12" cy="5" r="3"></circle></svg>' },
  { name: 'Garage', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2 20v-8a2 2 0 0 1 .41-1.21l8-10.69a2 2 0 0 1 3.18 0l8 10.69A2 2 0 0 1 22 12v8"></path><path d="M2 20h20"></path><path d="M6 20v-4a2 2 0 0 1 2-2h8a2 2 0 0 1 2 2v4"></path></svg>' },
  { name: 'Farm', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M3 22h18"></path><path d="M4 22V11l8-7 8 7v11"></path><path d="M9 22v-6h6v6"></path><path d="M12 7v4"></path></svg>' },
  { name: 'Penthouse', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2 22h20"></path><rect x="4" y="10" width="16" height="12" rx="2"></rect><path d="M4 10l8-8 8 8"></path><path d="M10 22v-4h4v4"></path></svg>' },
  { name: 'Studio', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><rect x="3" y="3" width="18" height="18" rx="2"></rect><path d="M3 9h18M9 21V9"></path></svg>' },
  { name: 'Cabin', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2 22h20"></path><path d="M4 22L12 6l8 16"></path><path d="M10 22v-6h4v6"></path></svg>' },
  { name: 'Mansion', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2 22h20"></path><path d="M3 22V9l9-7 9 7v13"></path><path d="M10 22v-4h4v4"></path><circle cx="12" cy="10" r="1"></circle><path d="M7 14h2M15 14h2"></path></svg>' },
  // Commercial & Industrial
  { name: 'Mall', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><rect x="2" y="7" width="20" height="14" rx="2"></rect><path d="M17 7V5a2 2 0 0 0-2-2H9a2 2 0 0 0-2 2v2"></path><path d="M12 12v5"></path><path d="M8 12v5M16 12v5"></path></svg>' },
  { name: 'Store', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2 3h20v5H2z"></path><path d="M2 8v13h20V8"></path><path d="M9 21v-8h6v8"></path></svg>' },
  { name: 'Restaurant', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M3 2v7c0 1.1.9 2 2 2h4a2 2 0 0 0 2-2V2"></path><path d="M7 2v20"></path><path d="M21 15V2a5 5 0 0 0-5 5v6c0 1.1.9 2 2 2h3Zm0 0v7"></path></svg>' },
  { name: 'Hospital', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><rect x="3" y="3" width="18" height="18" rx="2"></rect><path d="M9 3v18M15 3v18M3 9h18M3 15h18"></path></svg>' },
  { name: 'School', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M22 10v6M2 10l10-5 10 5-10 5z"></path><path d="M6 12v5c3 3 9 3 12 0v-5"></path></svg>' },
  { name: 'Bank', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M3 22h18"></path><path d="M6 18v-7M10 18v-7M14 18v-7M18 18v-7"></path><path d="M3 11h18L12 3z"></path></svg>' },
  { name: 'Church', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M10 22v-6h4v6"></path><path d="M4 22h16"></path><path d="M4 22v-8l8-8 8 8v8"></path><path d="M12 2v4M10 4h4"></path></svg>' },
  { name: 'Theater', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2 16.1A5 5 0 0 1 5.9 20M9.9 20a5 5 0 0 0 3.9-3.9M21.9 16.1A5 5 0 0 0 18.1 20"></path><path d="M12 2a8 8 0 0 0-8 8v8h16v-8a8 8 0 0 0-8-8z"></path><circle cx="12" cy="10" r="3"></circle></svg>' },
  { name: 'Stadium', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><ellipse cx="12" cy="12" rx="10" ry="5"></ellipse><path d="M2 12v5c0 2.76 4.48 5 10 5s10-2.24 10-5v-5"></path><path d="M12 7v10"></path></svg>' },
  // Amenities & Features
  { name: 'Pool', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2 12h20"></path><path d="M2 16c2.5 2 5.5 2 8 0 2.5-2 5.5-2 8 0 2.5 2 5.5 2 8 0"></path><path d="M6 12V4M18 12V4"></path><path d="M6 8h12"></path></svg>' },
  { name: 'Gym', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="m6.5 6.5 11 11"></path><path d="m21 21-1-1"></path><path d="m3 3 1 1"></path><path d="m18 22 4-4"></path><path d="m2 6 4-4"></path><path d="m3 10 7-7"></path><path d="m14 21 7-7"></path></svg>' },
  { name: 'Garden', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M12 22v-7"></path><path d="M7 15a5 5 0 0 1 10 0"></path><path d="M12 10a3 3 0 0 0 0-6"></path><path d="M12 10a3 3 0 0 1 0-6"></path><path d="M4 22h16"></path></svg>' },
  { name: 'Parking', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><rect x="3" y="3" width="18" height="18" rx="2"></rect><path d="M9 17V7h4a3 3 0 0 1 0 6H9"></path></svg>' },
  { name: 'Elevator', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><rect x="4" y="2" width="16" height="20" rx="2"></rect><path d="M12 6v6"></path><path d="m9 9 3-3 3 3"></path><path d="M12 12v6"></path><path d="m9 15 3 3 3-3"></path></svg>' },
  { name: 'Security', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"></path><path d="m9 12 2 2 4-4"></path></svg>' },
  { name: 'Wifi', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M5 13a10 10 0 0 1 14 0"></path><path d="M8.5 16.5a5 5 0 0 1 7 0"></path><path d="M2 9.5a14 14 0 0 1 20 0"></path><circle cx="12" cy="20" r="1"></circle></svg>' },
  { name: 'AC', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M12 3v6"></path><path d="m6.3 8.3 3 3"></path><path d="m14.7 11.3 3-3"></path><path d="M4 14h16"></path><path d="M4 18h16"></path></svg>' },
  // Nature & Outdoor
  { name: 'Mountain', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="m8 3 4 8 5-5 5 15H2L8 3z"></path></svg>' },
  { name: 'Beach', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2 22h20"></path><path d="M12 2v8"></path><path d="M5 10a7 7 0 0 0 14 0"></path><path d="M5 18h14"></path></svg>' },
  { name: 'Lake', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2 14c2-2 4-2 6 0 2 2 4 2 6 0 2-2 4-2 6 0 2 2 4 2 6 0"></path><path d="M2 18c2-2 4-2 6 0 2 2 4 2 6 0 2-2 4-2 6 0 2 2 4 2 6 0"></path><path d="M2 10c2-2 4-2 6 0 2 2 4 2 6 0 2-2 4-2 6 0 2 2 4 2 6 0"></path></svg>' },
  { name: 'Forest', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M10 10v.2A3 3 0 0 1 8.9 16H5a3 3 0 0 1 0-6h1.1a3 3 0 0 1 2-4.6 5 5 0 0 1 9.8 1.6"></path><path d="M14 10v.2a3 3 0 0 0 1.1 2.3H19a3 3 0 0 0 0-6h-1.1a3 3 0 0 0-2-4.6"></path><path d="M8 16v6M16 16v6M12 22V12"></path></svg>' },
  // Transportation
  { name: 'Boat', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2 21c.6.5 1.2 1 2.5 1 2.5 0 2.5-2 5-2 1.3 0 1.9.5 2.5 1 .6.5 1.2 1 2.5 1 2.5 0 2.5-2 5-2 1.3 0 1.9.5 2.5 1"></path><path d="M19.38 20A11.6 11.6 0 0 0 21 14l-9-4-9 4c0 2.9.94 5.34 2.81 7.76"></path><path d="M19 13V7a2 2 0 0 0-2-2H7a2 2 0 0 0-2 2v6"></path><path d="M12 10v4"></path></svg>' },
  { name: 'Train', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><rect x="4" y="3" width="16" height="16" rx="2"></rect><path d="M4 11h16"></path><path d="M12 3v8"></path><path d="m8 19-2 3M16 19l2 3"></path><path d="M8 15h0M16 15h0"></path></svg>' },
  { name: 'Airport', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M17.8 19.2 16 11l3.5-3.5C21 6 21.5 4 21 3c-1-.5-3 0-4.5 1.5L13 8 4.8 6.2c-.5-.1-.9.1-1.1.5l-.3.5c-.2.5-.1 1 .3 1.3L9 12l-2 3H4l-1 1 3 2 2 3 1-1v-3l3-2 3.5 5.3c.3.4.8.5 1.3.3l.5-.2c.4-.3.6-.7.5-1.2z"></path></svg>' },
  // Others
  { name: 'Key', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="m21 2-2 2m-7.61 7.61a5.5 5.5 0 1 1-7.778 7.778 5.5 5.5 0 0 1 7.777-7.777zm0 0L15.5 7.5m0 0 3 3L22 7l-3-3m-3.5 3.5L19 4"></path></svg>' },
  { name: 'Location', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z"></path><circle cx="12" cy="10" r="3"></circle></svg>' },
  { name: 'Star', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"></polygon></svg>' },
  { name: 'Heart', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z"></path></svg>' },
  { name: 'Crown', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="m2 4 3 12h14l3-12-6 7-4-7-4 7-6-7zm3 16h14"></path></svg>' },
  { name: 'Diamond', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-8 h-8"><path d="M2.7 10.3a2.41 2.41 0 0 0 0 3.41l7.59 7.59a2.41 2.41 0 0 0 3.41 0l7.59-7.59a2.41 2.41 0 0 0 0-3.41l-7.59-7.59a2.41 2.41 0 0 0-3.41 0Z"></path></svg>' }
]

const defaultPropertyType = {
  id: null,
  name: '',
  icon: availableIcons[0].svg,
  banner: ''
}

const propertyTypeForm = ref({ ...defaultPropertyType })

const openPropertyTypeModal = () => {
  isEditingType.value = false
  propertyTypeForm.value = { ...defaultPropertyType }
  showPropertyTypeModal.value = true
}

const editPropertyType = (type) => {
  isEditingType.value = true
  propertyTypeForm.value = { ...type }
  showPropertyTypeModal.value = true
}

const closePropertyTypeModal = () => {
  showPropertyTypeModal.value = false
}

const handleBannerUpload = (event) => {
  const file = event.target.files[0]
  if (file) {
    propertyTypeForm.value.banner = URL.createObjectURL(file)
  }
}

const selectIcon = (svg) => {
  propertyTypeForm.value.icon = svg
}

const savePropertyType = () => {
  if (!propertyTypeForm.value.name || !propertyTypeForm.value.banner) {
    alert('กรุณากรอกข้อมูลให้ครบถ้วน')
    return
  }

  if (isEditingType.value) {
    const index = propertyTypes.value.findIndex(t => t.id === propertyTypeForm.value.id)
    if (index !== -1) {
      propertyTypes.value[index] = { ...propertyTypeForm.value }
    }
  } else {
    propertyTypes.value.push({
      ...propertyTypeForm.value,
      id: Date.now(),
      count: 0
    })
  }
  closePropertyTypeModal()
}

const removePropertyType = (index) => {
  if (confirm('ต้องการลบประเภทอสังหานี้ใช่หรือไม่?')) {
    propertyTypes.value.splice(index, 1)
  }
}

// Menu Management
const showMenuModal = ref(false)
const isEditingMenu = ref(false)
const menuItems = ref([
  { 
    id: 1, 
    name: 'หน้าแรก', 
    url: '/',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg>',
    isActive: true,
    order: 1
  },
  { 
    id: 2, 
    name: 'อสังหาริมทรัพย์', 
    url: '/properties',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><rect x="4" y="2" width="16" height="20" rx="2" ry="2"></rect><line x1="9" y1="6" x2="9" y2="6.01"></line><line x1="15" y1="6" x2="15" y2="6.01"></line><line x1="9" y1="10" x2="9" y2="10.01"></line><line x1="15" y1="10" x2="15" y2="10.01"></line><line x1="9" y1="14" x2="9" y2="14.01"></line><line x1="15" y1="14" x2="15" y2="14.01"></line><path d="M9 18h6v4H9z"></path></svg>',
    isActive: true,
    order: 2
  },
  { 
    id: 3, 
    name: 'เกี่ยวกับเรา', 
    url: '/about',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="16" x2="12" y2="12"></line><line x1="12" y1="8" x2="12.01" y2="8"></line></svg>',
    isActive: true,
    order: 3
  },
  { 
    id: 4, 
    name: 'ติดต่อ', 
    url: '/contact',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"></path></svg>',
    isActive: true,
    order: 4
  }
])

const menuIcons = [
  { name: 'Home', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg>' },
  { name: 'Building', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><rect x="4" y="2" width="16" height="20" rx="2" ry="2"></rect><line x1="9" y1="6" x2="9" y2="6.01"></line><line x1="15" y1="6" x2="15" y2="6.01"></line><line x1="9" y1="10" x2="9" y2="10.01"></line><line x1="15" y1="10" x2="15" y2="10.01"></line><line x1="9" y1="14" x2="9" y2="14.01"></line><line x1="15" y1="14" x2="15" y2="14.01"></line><path d="M9 18h6v4H9z"></path></svg>' },
  { name: 'Info', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="16" x2="12" y2="12"></line><line x1="12" y1="8" x2="12.01" y2="8"></line></svg>' },
  { name: 'Phone', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"></path></svg>' },
  { name: 'Mail', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path><polyline points="22,6 12,13 2,6"></polyline></svg>' },
  { name: 'User', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path><circle cx="12" cy="7" r="4"></circle></svg>' },
  { name: 'Users', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="9" cy="7" r="4"></circle><path d="M23 21v-2a4 4 0 0 0-3-3.87"></path><path d="M16 3.13a4 4 0 0 1 0 7.75"></path></svg>' },
  { name: 'Search', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><circle cx="11" cy="11" r="8"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>' },
  { name: 'Heart', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"></path></svg>' },
  { name: 'Star', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"></polygon></svg>' },
  { name: 'Map', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path><circle cx="12" cy="10" r="3"></circle></svg>' },
  { name: 'File', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path><polyline points="14 2 14 8 20 8"></polyline></svg>' },
  { name: 'Image', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"></rect><circle cx="8.5" cy="8.5" r="1.5"></circle><polyline points="21 15 16 10 5 21"></polyline></svg>' },
  { name: 'Video', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><polygon points="23 7 16 12 23 17 23 7"></polygon><rect x="1" y="5" width="15" height="14" rx="2" ry="2"></rect></svg>' },
  { name: 'Cart', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><circle cx="9" cy="21" r="1"></circle><circle cx="20" cy="21" r="1"></circle><path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path></svg>' },
  { name: 'Tag', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M20.59 13.41l-7.17 7.17a2 2 0 0 1-2.83 0L2 12V2h10l8.59 8.59a2 2 0 0 1 0 2.82z"></path><line x1="7" y1="7" x2="7.01" y2="7"></line></svg>' },
  { name: 'Calendar', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg>' },
  { name: 'Clock', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><circle cx="12" cy="12" r="10"></circle><polyline points="12 6 12 12 16 14"></polyline></svg>' },
  { name: 'Settings', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><circle cx="12" cy="12" r="3"></circle><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"></path></svg>' },
  { name: 'Help', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><circle cx="12" cy="12" r="10"></circle><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"></path><line x1="12" y1="17" x2="12.01" y2="17"></line></svg>' },
  { name: 'Book', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"></path><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"></path></svg>' },
  { name: 'Award', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><circle cx="12" cy="8" r="7"></circle><polyline points="8.21 13.89 7 23 12 20 17 23 15.79 13.88"></polyline></svg>' },
  { name: 'Briefcase', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><rect x="2" y="7" width="20" height="14" rx="2" ry="2"></rect><path d="M16 21V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v16"></path></svg>' },
  { name: 'Globe', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><circle cx="12" cy="12" r="10"></circle><line x1="2" y1="12" x2="22" y2="12"></line><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"></path></svg>' },
  { name: 'Link', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg>' },
  { name: 'Shield', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"></path></svg>' },
  { name: 'Zap', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><polygon points="13 2 3 14 12 14 11 22 21 10 12 10 13 2"></polygon></svg>' },
  { name: 'Gift', svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><polyline points="20 12 20 22 4 22 4 12"></polyline><rect x="2" y="7" width="20" height="5"></rect><line x1="12" y1="22" x2="12" y2="7"></line><path d="M12 7H7.5a2.5 2.5 0 0 1 0-5C11 2 12 7 12 7z"></path><path d="M12 7h4.5a2.5 2.5 0 0 0 0-5C13 2 12 7 12 7z"></path></svg>' }
]

const defaultMenuItem = {
  id: null,
  name: '',
  url: '',
  icon: menuIcons[0].svg,
  isActive: true,
  order: 0
}

const menuForm = ref({ ...defaultMenuItem })

const openMenuModal = () => {
  isEditingMenu.value = false
  menuForm.value = { ...defaultMenuItem, order: menuItems.value.length + 1 }
  showMenuModal.value = true
}

const editMenuItem = (item) => {
  isEditingMenu.value = true
  menuForm.value = { ...item }
  showMenuModal.value = true
}

const closeMenuModal = () => {
  showMenuModal.value = false
}

const selectMenuIcon = (svg) => {
  menuForm.value.icon = svg
}

const saveMenuItem = () => {
  if (!menuForm.value.name || !menuForm.value.url) {
    alert('กรุณากรอกชื่อเมนูและ URL')
    return
  }

  if (isEditingMenu.value) {
    const index = menuItems.value.findIndex(m => m.id === menuForm.value.id)
    if (index !== -1) {
      menuItems.value[index] = { ...menuForm.value }
    }
  } else {
    menuItems.value.push({
      ...menuForm.value,
      id: Date.now()
    })
  }
  closeMenuModal()
}

const removeMenuItem = (index) => {
  if (confirm('ต้องการลบเมนูนี้ใช่หรือไม่?')) {
    menuItems.value.splice(index, 1)
  }
}

const toggleMenuStatus = (index) => {
  menuItems.value[index].isActive = !menuItems.value[index].isActive
}

const moveMenuUp = (index) => {
  if (index > 0) {
    const temp = menuItems.value[index]
    menuItems.value[index] = menuItems.value[index - 1]
    menuItems.value[index - 1] = temp
  }
}

const moveMenuDown = (index) => {
  if (index < menuItems.value.length - 1) {
    const temp = menuItems.value[index]
    menuItems.value[index] = menuItems.value[index + 1]
    menuItems.value[index + 1] = temp
  }
}

// Country Management
const showCountryModal = ref(false)
const countries = ref([
  { name: 'Thailand', code: 'th', flag: '🇹🇭', language: 'Thai', currencyCode: 'THB', currencySymbol: '฿', currencyName: 'Thai Baht' },
  { name: 'United States', code: 'en', flag: '🇺🇸', language: 'English', currencyCode: 'USD', currencySymbol: '$', currencyName: 'US Dollar' },
  { name: 'China', code: 'cn', flag: '🇨🇳', language: 'Chinese', currencyCode: 'CNY', currencySymbol: '¥', currencyName: 'Chinese Yuan' }
])

const defaultNewCountry = {
  name: '',
  code: '',
  flag: '',
  language: '',
  currencyCode: '',
  currencySymbol: '',
  currencyName: ''
}

const newCountry = ref({ ...defaultNewCountry })

const openAddCountryModal = () => {
  newCountry.value = { ...defaultNewCountry }
  showCountryModal.value = true
}

const closeCountryModal = () => {
  showCountryModal.value = false
}

const addCountry = () => {
  if (!newCountry.value.name || !newCountry.value.code) {
    alert('กรุณากรอกข้อมูลให้ครบถ้วน')
    return
  }
  countries.value.push({ ...newCountry.value })
  closeCountryModal()
}

const removeCountry = (index) => {
  if (confirm('ต้องการลบประเทศนี้ใช่หรือไม่?')) {
    countries.value.splice(index, 1)
  }
}

const seo = reactive({
  defaultTitle: 'DDexpert Condo - คอนโดมือสองคุณภาพ',
  metaDescription: 'ศูนย์รวมคอนโดมือสองคุณภาพเยี่ยม ราคาดีที่สุด พร้อมบริการให้คำปรึกษาด้านสินเชื่อและการลงทุน',
  keywords: 'คอนโด, คอนโดมือสอง, ซื้อคอนโด, ขายคอนโด, ลงทุนคอนโด, อสังหาริมทรัพย์',
  ogImage: 'https://images.unsplash.com/photo-1560448204-e02f11c3d0e2?w=1200&h=630&fit=crop',
  fbAppId: '',
  twitterHandle: '@ddexpert',
  robotsTxt: 'User-agent: *\nAllow: /\nDisallow: /admin/'
})

const handleOgImageUpload = (event) => {
  const file = event.target.files[0]
  if (file) {
    seo.ogImage = URL.createObjectURL(file)
  }
}

const removeOgImage = () => {
  seo.ogImage = ''
}

const saveSettings = () => {
  // Simulate API call
  console.log('Settings saved:', { seo, localization, countries: countries.value })
  alert('บันทึกการตั้งค่าเรียบร้อยแล้ว')
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

.custom-scrollbar::-webkit-scrollbar {
  width: 6px;
}
.custom-scrollbar::-webkit-scrollbar-track {
  background: #f1f5f9;
  border-radius: 4px;
}
.custom-scrollbar::-webkit-scrollbar-thumb {
  background: #cbd5e1;
  border-radius: 4px;
}
.custom-scrollbar::-webkit-scrollbar-thumb:hover {
  background: #94a3b8;
}
</style>

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
                <svg v-if="tab.id === 'seo'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><circle cx="12" cy="12" r="10"/><line x1="2" y1="12" x2="22" y2="12"/><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/></svg>
                <svg v-if="tab.id === 'social'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><path d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"/></svg>
                <svg v-if="tab.id === 'advanced'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><polyline points="16 18 22 12 16 6"/><polyline points="8 6 2 12 8 18"/></svg>
              </span>
              {{ tab.name }}
            </button>
          </nav>

            <!-- Localization Settings -->
            <div v-if="activeTab === 'general'" class="space-y-6">
              <!-- Default Settings -->
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
  { id: 'seo', name: 'SEO' },
  { id: 'social', name: 'Social Media' },
  { id: 'advanced', name: 'Advanced' }
]

const localization = reactive({
  language: 'th',
  currency: 'THB'
})

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
</style>

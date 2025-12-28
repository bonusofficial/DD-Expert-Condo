<template>
  <div class="min-h-screen bg-white font-sans text-slate-600 pb-20">
    <Navbar />

    <!-- Main Container -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      
      <!-- Top Header: Title & Actions -->
      <div class="flex flex-col md:flex-row md:items-start md:justify-between gap-4 mb-6">
        <div class="space-y-2">
          <h1 class="text-2xl md:text-3xl font-bold text-slate-900 leading-tight">
            {{ property.title }}
          </h1>
          <div class="flex items-center gap-2 text-slate-500 text-sm md:text-base">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5 text-[#DAA520]">
              <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
              <circle cx="12" cy="10" r="3"/>
            </svg>
            <span>{{ property.location }}</span>
            <span class="w-1 h-1 bg-slate-300 rounded-full mx-1"></span>
            <span class="text-[#DAA520] font-medium">{{ property.projectName }}</span>
          </div>
        </div>
        
        <div class="flex items-center gap-3">
          <button class="flex items-center gap-2 px-4 py-2 rounded-lg border border-slate-200 hover:bg-slate-50 hover:border-[#DAA520] hover:text-[#DAA520] transition-all text-sm font-medium">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><path d="M4 12v8a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2v-8"/><polyline points="16 6 12 2 8 6"/><line x1="12" y1="2" x2="12" y2="15"/></svg>
            แชร์
          </button>
          <button class="flex items-center gap-2 px-4 py-2 rounded-lg border border-slate-200 hover:bg-slate-50 hover:border-red-500 hover:text-red-500 transition-all text-sm font-medium">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-4 h-4"><path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/></svg>
            บันทึก
          </button>
        </div>
      </div>

      <!-- Image Mosaic Gallery -->
      <div class="grid grid-cols-1 md:grid-cols-4 gap-2 h-[300px] md:h-[450px] rounded-2xl overflow-hidden mb-8 relative group cursor-pointer" @click="openImageModal">
        <!-- Main Image (Left) -->
        <div class="md:col-span-2 md:row-span-2 relative h-full">
          <img :src="property.images[0]" class="w-full h-full object-cover hover:scale-105 transition-transform duration-700" />
        </div>
        <!-- Sub Images (Right) -->
        <div class="hidden md:block relative h-full">
          <img :src="property.images[1]" class="w-full h-full object-cover hover:scale-105 transition-transform duration-700" />
        </div>
        <div class="hidden md:block relative h-full">
          <img :src="property.images[2]" class="w-full h-full object-cover hover:scale-105 transition-transform duration-700" />
        </div>
        <div class="hidden md:block relative h-full">
          <img :src="property.images[3]" class="w-full h-full object-cover hover:scale-105 transition-transform duration-700" />
        </div>
        <div class="hidden md:block relative h-full">
          <img :src="property.images[4]" class="w-full h-full object-cover hover:scale-105 transition-transform duration-700" />
          <!-- View All Button Overlay -->
          <div class="absolute inset-0 bg-black/40 flex items-center justify-center hover:bg-black/50 transition-colors">
             <span class="text-white font-medium flex items-center gap-2">
               <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/></svg>
               ดูรูปทั้งหมด ({{ property.images.length }})
             </span>
          </div>
        </div>
        
        <!-- Mobile Badge -->
        <div class="md:hidden absolute bottom-4 right-4 bg-black/60 text-white px-3 py-1 rounded-full text-sm backdrop-blur-sm">
          {{ property.images.length }} รูปภาพ
        </div>
      </div>

      <!-- Content Layout -->
      <div class="grid grid-cols-1 lg:grid-cols-[1fr_380px] gap-10">
        
        <!-- Left Column: Details -->
        <div class="space-y-10">
          
          <!-- Key Specs -->
          <div class="flex flex-wrap gap-4 pb-8 border-b border-slate-100">
            <div class="flex items-center gap-3 px-4 py-3 bg-slate-50 rounded-xl border border-slate-100">
              <div class="p-2 bg-white rounded-lg shadow-sm text-[#DAA520]">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M2 4v16M2 8h18a2 2 0 0 1 2 2v10M2 17h20M6 8v9"/></svg>
              </div>
              <div>
                <div class="font-bold text-slate-900">{{ property.bedrooms }} ห้องนอน</div>
                <div class="text-xs text-slate-500">Bedrooms</div>
              </div>
            </div>
            <div class="flex items-center gap-3 px-4 py-3 bg-slate-50 rounded-xl border border-slate-100">
              <div class="p-2 bg-white rounded-lg shadow-sm text-[#DAA520]">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M9 6 6.5 3.5a1.5 1.5 0 0 0-1-.5C4.683 3 4 3.683 4 4.5V17a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2v-5"/></svg>
              </div>
              <div>
                <div class="font-bold text-slate-900">{{ property.bathrooms }} ห้องน้ำ</div>
                <div class="text-xs text-slate-500">Bathrooms</div>
              </div>
            </div>
            <div class="flex items-center gap-3 px-4 py-3 bg-slate-50 rounded-xl border border-slate-100">
              <div class="p-2 bg-white rounded-lg shadow-sm text-[#DAA520]">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"/></svg>
              </div>
              <div>
                <div class="font-bold text-slate-900">{{ property.size }} ตร.ม.</div>
                <div class="text-xs text-slate-500">Size</div>
              </div>
            </div>
            <div class="flex items-center gap-3 px-4 py-3 bg-slate-50 rounded-xl border border-slate-100">
              <div class="p-2 bg-white rounded-lg shadow-sm text-[#DAA520]">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><rect x="4" y="2" width="16" height="20" rx="2" ry="2"/><line x1="4" y1="10" x2="20" y2="10"/></svg>
              </div>
              <div>
                <div class="font-bold text-slate-900">ชั้น {{ property.floor }}</div>
                <div class="text-xs text-slate-500">Floor</div>
              </div>
            </div>
          </div>

          <!-- Description -->
          <div class="space-y-4">
            <h2 class="text-xl font-bold text-slate-900">รายละเอียดทรัพย์</h2>
            <div class="prose prose-slate max-w-none text-slate-600 leading-relaxed">
              <p>{{ property.description }}</p>
            </div>
            
            <div class="grid grid-cols-2 md:grid-cols-3 gap-y-4 gap-x-8 py-6 border-y border-slate-100 mt-6">
              <div>
                <span class="block text-sm text-slate-500 mb-1">รหัสทรัพย์</span>
                <span class="font-medium text-slate-900">{{ property.id }}</span>
              </div>
              <div>
                <span class="block text-sm text-slate-500 mb-1">ประเภท</span>
                <span class="font-medium text-slate-900">{{ property.propertyType }}</span>
              </div>
              <div>
                <span class="block text-sm text-slate-500 mb-1">โครงการ</span>
                <span class="font-medium text-slate-900">{{ property.projectName }}</span>
              </div>
              <div>
                <span class="block text-sm text-slate-500 mb-1">ลงประกาศเมื่อ</span>
                <span class="font-medium text-slate-900">{{ property.postedDate }}</span>
              </div>
              <div>
                <span class="block text-sm text-slate-500 mb-1">อาคาร</span>
                <span class="font-medium text-slate-900">{{ property.building }}</span>
              </div>
              <div>
                <span class="block text-sm text-slate-500 mb-1">ทิศ</span>
                <span class="font-medium text-slate-900">{{ property.direction }}</span>
              </div>
            </div>
          </div>

          <!-- Facilities -->
          <div class="space-y-4">
            <h2 class="text-xl font-bold text-slate-900">สิ่งอำนวยความสะดวก</h2>
            <div class="grid grid-cols-2 md:grid-cols-3 gap-4">
              <div v-for="item in property.facilities" :key="item" class="flex items-center gap-3 p-3 rounded-lg bg-slate-50 text-slate-700">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5 text-emerald-500"><polyline points="20 6 9 17 4 12"/></svg>
                <span class="text-sm font-medium">{{ item }}</span>
              </div>
            </div>
          </div>

          <!-- Amenities -->
           <div class="space-y-4">
            <h2 class="text-xl font-bold text-slate-900">สิ่งที่มีให้ในห้อง</h2>
            <div class="grid grid-cols-2 md:grid-cols-3 gap-4">
              <div v-for="item in property.amenities" :key="item" class="flex items-center gap-3 p-3 rounded-lg bg-slate-50 text-slate-700">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5 text-emerald-500"><polyline points="20 6 9 17 4 12"/></svg>
                <span class="text-sm font-medium">{{ item }}</span>
              </div>
            </div>
          </div>

          <!-- Location / Nearby -->
          <div class="space-y-4">
            <h2 class="text-xl font-bold text-slate-900">ทำเลที่ตั้ง</h2>
            <div class="bg-slate-100 rounded-2xl h-[300px] flex items-center justify-center text-slate-400">
              <!-- Map Placeholder -->
              <span>Map Component Here</span>
            </div>
            
            <div class="mt-6">
              <div class="flex gap-2 overflow-x-auto pb-2 mb-4">
                 <button 
                  v-for="tab in nearbyTabs"
                  :key="tab.value"
                  :class="['px-4 py-2 rounded-full text-sm font-medium whitespace-nowrap transition-all', activeNearbyTab === tab.value ? 'bg-[#DAA520] text-white shadow-lg shadow-amber-500/20' : 'bg-slate-100 text-slate-600 hover:bg-slate-200']"
                  @click="activeNearbyTab = tab.value"
                >
                  {{ tab.label }}
                </button>
              </div>
              <div class="space-y-3">
                <div v-for="place in filteredNearbyPlaces" :key="place.name" class="flex items-center justify-between p-3 border-b border-slate-50 last:border-0">
                  <span class="text-slate-700">{{ place.name }}</span>
                  <span class="text-sm text-slate-500 font-medium">{{ place.distance }}</span>
                </div>
              </div>
            </div>
          </div>

        </div>

        <!-- Right Column: Sticky Sidebar -->
        <div class="relative">
          <div class="sticky top-24 space-y-6">
            
            <!-- Price Card -->
            <div class="bg-white rounded-2xl shadow-xl shadow-slate-200/50 border border-slate-100 p-6">
              <div class="flex items-center justify-between mb-2">
                <span class="px-3 py-1 rounded-md bg-emerald-50 text-emerald-600 text-sm font-bold">
                  {{ property.listingType }}
                </span>
                <span class="text-sm text-slate-400">อัปเดตล่าสุด: {{ property.postedDate }}</span>
              </div>
              <div class="text-3xl font-bold text-[#DAA520] mb-1">฿ {{ formatPrice(property.price) }}</div>
              <div class="text-sm text-slate-500 mb-6">
                ราคาเฉลี่ย {{ formatPrice(Math.round(property.price / property.size)) }} บาท/ตร.ม.
              </div>
              
              <div class="space-y-3">
                <button class="w-full py-3.5 rounded-xl bg-[#DAA520] text-white font-bold shadow-lg shadow-amber-500/20 hover:bg-[#b8860b] hover:shadow-amber-500/30 hover:-translate-y-0.5 transition-all flex items-center justify-center gap-2">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72"/></svg>
                  โทรติดต่อ
                </button>
                <button class="w-full py-3.5 rounded-xl border-2 border-[#DAA520] text-[#DAA520] font-bold hover:bg-amber-50 transition-all flex items-center justify-center gap-2">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-5 h-5"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>
                  ทักแชทไลน์
                </button>
              </div>
            </div>

            <!-- Agent Card -->
            <div class="bg-white rounded-2xl shadow-lg shadow-slate-200/50 border border-slate-100 p-6">
              <div class="flex items-center gap-4 mb-6">
                <img :src="property.agent.avatar" class="w-16 h-16 rounded-full object-cover border-2 border-slate-100" />
                <div>
                  <div class="font-bold text-slate-900 text-lg">{{ property.agent.name }}</div>
                  <div class="text-sm text-slate-500">{{ property.agent.company }}</div>
                  <div class="flex items-center gap-1 text-amber-500 text-xs mt-1">
                    <svg v-for="i in 5" :key="i" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-3 h-3"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>
                    <span class="text-slate-400 ml-1">(4.9)</span>
                  </div>
                </div>
              </div>
              
              <div class="space-y-3">
                <input type="text" v-model="contactForm.name" placeholder="ชื่อ-นามสกุลของคุณ" class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all text-sm" />
                <input type="tel" v-model="contactForm.phone" placeholder="เบอร์โทรศัพท์" class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all text-sm" />
                <textarea v-model="contactForm.message" rows="3" class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-[#DAA520]/20 focus:border-[#DAA520] outline-none transition-all text-sm resize-none"></textarea>
                <button class="w-full py-3 rounded-xl bg-slate-900 text-white font-bold hover:bg-slate-800 transition-all shadow-lg shadow-slate-900/20">
                  ส่งข้อความ
                </button>
              </div>
            </div>

          </div>
        </div>

      </div>
    </main>

    <!-- Image Modal (Keep existing logic but style it) -->
    <Teleport to="body">
      <div v-if="isModalOpen" class="fixed inset-0 z-[9999] bg-black/95 flex flex-col" @click="closeImageModal">
        <!-- Modal Header -->
        <div class="flex justify-between items-center p-4 text-white bg-black/50 backdrop-blur-sm">
          <div class="font-medium">{{ currentImageIndex + 1 }} / {{ property.images.length }}</div>
          <button @click="closeImageModal" class="p-2 hover:bg-white/10 rounded-full transition-colors">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-6 h-6"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
          </button>
        </div>
        
        <!-- Main Image -->
        <div class="flex-1 flex items-center justify-center relative p-4" @click.stop>
          <button class="absolute left-4 p-3 rounded-full bg-black/50 text-white hover:bg-black/80 transition-all" @click="prevImage">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-6 h-6"><polyline points="15 18 9 12 15 6"/></svg>
          </button>
          
          <img :src="currentImage" class="max-h-full max-w-full object-contain rounded-lg shadow-2xl" />
          
          <button class="absolute right-4 p-3 rounded-full bg-black/50 text-white hover:bg-black/80 transition-all" @click="nextImage">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="w-6 h-6"><polyline points="9 18 15 12 9 6"/></svg>
          </button>
        </div>

        <!-- Thumbnails -->
        <div class="p-4 flex justify-center gap-2 overflow-x-auto" @click.stop>
          <button 
            v-for="(image, index) in property.images" 
            :key="index"
            :class="['w-16 h-12 rounded-md overflow-hidden border-2 transition-all flex-shrink-0', currentImageIndex === index ? 'border-[#DAA520] opacity-100' : 'border-transparent opacity-50 hover:opacity-80']"
            @click="currentImageIndex = index"
          >
            <img :src="image" class="w-full h-full object-cover" />
          </button>
        </div>
      </div>
    </Teleport>

    <Footer />
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();

// Current image index for gallery
const currentImageIndex = ref(0);

// Tab states
const inquiryType = ref('message');
const activeNearbyTab = ref('transport');

// Contact form
const contactForm = ref({
  name: '',
  phone: '',
  message: 'สนใจทรัพย์รายการนี้ ต้องการข้อมูลเพิ่มเติม'
});

// Sample property data
const property = ref({
  id: 'DDX-25-001',
  title: 'ดาวดี เดอไลท์ พหลโยธิน 25 2 ห้องนอน ห้องกว้าง เพิ่งปล่อยเช่าพร้อมเข้าอยู่ ติด MRT พหลฯ',
  listingType: 'ขาย',
  propertyType: 'คอนโด',
  projectName: 'ดาวดี เดอไลท์ พหลโยธิน 25',
  location: 'พหลโยธิน, จตุจักร, กรุงเทพมหานคร',
  price: 9990000,
  size: 72.04,
  bedrooms: 2,
  bathrooms: 2,
  floor: 15,
  building: 'A',
  direction: 'ทิศเหนือ',
  postedDate: '15/12/2025',
  description: 'คอนโดหรูใจกลางเมือง ใกล้ BTS และ MRT เดินทางสะดวก ห้องกว้างขวาง ตกแต่งครบ พร้อมเข้าอยู่ วิวสวย ไม่บล็อค เหมาะสำหรับครอบครัวหรือการลงทุน ปล่อยเช่าได้ผลตอบแทนดี',
  images: [
    'https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=800',
    'https://images.unsplash.com/photo-1560448204-e02f11c3d0e2?w=800',
    'https://images.unsplash.com/photo-1484154218962-a197022b5858?w=800',
    'https://images.unsplash.com/photo-1560185007-c5ca9d2c014d?w=800',
    'https://images.unsplash.com/photo-1560185009-5bf9f2849488?w=800',
  ],
  amenities: ['เฟอร์นิเจอร์ครบ', 'เครื่องปรับอากาศ', 'เครื่องทำน้ำอุ่น', 'ตู้เย็น', 'ทีวี', 'เครื่องซักผ้า'],
  facilities: ['สระว่ายน้ำ', 'ฟิตเนส', 'ที่จอดรถ', 'รปภ. 24 ชม.', 'ลิฟต์', 'สวนหย่อม'],
  nearbyPlaces: [
    { type: 'transport', name: 'MRT พหลโยธิน', distance: '150 ม.' },
    { type: 'transport', name: 'BTS หมอชิต', distance: '500 ม.' },
    { type: 'transport', name: 'สถานีขนส่งหมอชิต', distance: '800 ม.' },
    { type: 'shopping', name: 'เซ็นทรัล ลาดพร้าว', distance: '1.2 กม.' },
    { type: 'shopping', name: 'ยูเนี่ยน มอลล์', distance: '300 ม.' },
    { type: 'education', name: 'มหาวิทยาลัยเกษตรศาสตร์', distance: '2 กม.' },
    { type: 'hospital', name: 'โรงพยาบาลพญาไท 2', distance: '1.5 กม.' },
  ],
  agent: {
    name: 'Mr.Best RealHome',
    company: 'DD Expert Condo',
    avatar: 'https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?w=150',
    phone: '02-XXX-XXXX'
  }
});

// Nearby tabs
const nearbyTabs = [
  { value: 'transport', label: 'สถานีรถไฟฟ้า' },
  { value: 'shopping', label: 'แหล่งช็อปปิ้ง' },
  { value: 'education', label: 'สถานศึกษา' },
  { value: 'hospital', label: 'สิ่งอำนวยความสะดวก' },
  { value: 'restaurant', label: 'ร้านอาหาร' },
];

// Computed
const currentImage = computed(() => property.value.images[currentImageIndex.value]);

const filteredNearbyPlaces = computed(() => {
  return property.value.nearbyPlaces.filter(place => place.type === activeNearbyTab.value);
});

// Methods
const formatPrice = (price) => {
  return new Intl.NumberFormat('th-TH').format(price);
};

const prevImage = () => {
  if (currentImageIndex.value > 0) {
    currentImageIndex.value--;
  } else {
    currentImageIndex.value = property.value.images.length - 1;
  }
};

const nextImage = () => {
  if (currentImageIndex.value < property.value.images.length - 1) {
    currentImageIndex.value++;
  } else {
    currentImageIndex.value = 0;
  }
};

// Image Modal
const isModalOpen = ref(false);

const openImageModal = () => {
  isModalOpen.value = true;
  document.body.style.overflow = 'hidden';
};

const closeImageModal = () => {
  isModalOpen.value = false;
  document.body.style.overflow = '';
};

// Keyboard navigation for modal
const handleKeydown = (e) => {
  if (!isModalOpen.value) return;
  
  if (e.key === 'Escape') {
    closeImageModal();
  } else if (e.key === 'ArrowLeft') {
    prevImage();
  } else if (e.key === 'ArrowRight') {
    nextImage();
  }
};

onMounted(() => {
  document.addEventListener('keydown', handleKeydown);
});

onUnmounted(() => {
  document.removeEventListener('keydown', handleKeydown);
  document.body.style.overflow = '';
});
</script>

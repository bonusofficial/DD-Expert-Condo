<template>
  <div class="condo-page">
    <Navbar />
    
    <!-- Page Header -->
    <section class="page-header">
      <div class="container">
        <div class="header-content">
          <h1 class="page-title">คอนโดมิเนียม</h1>
          <p class="page-subtitle">ค้นหาคอนโดในฝันของคุณ จากโครงการชั้นนำทั่วประเทศ</p>
        </div>
      </div>
    </section>

    <!-- Filter Section -->
    <section class="filter-section">
      <div class="container">
        <div class="filter-card">
          <div class="filter-row">
            <!-- Zone Filter -->
            <div class="filter-item">
              <select v-model="selectedZone" class="filter-select">
                <option value="">เลือกโซน</option>
                <option v-for="zone in zones" :key="zone.value" :value="zone.value">
                  {{ zone.label }}
                </option>
              </select>
              <svg class="select-arrow" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>

            <!-- Listing Type Filter -->
            <div class="filter-item">
              <select v-model="selectedListingType" class="filter-select">
                <option value="">ประเภทประกาศ</option>
                <option v-for="type in listingTypes" :key="type.value" :value="type.value">
                  {{ type.label }}
                </option>
              </select>
              <svg class="select-arrow" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>

            <!-- Room Count Filter -->
            <div class="filter-item">
              <select v-model="selectedRooms" class="filter-select">
                <option value="">จำนวนห้องนอน</option>
                <option v-for="room in roomOptions" :key="room.value" :value="room.value">
                  {{ room.label }}
                </option>
              </select>
              <svg class="select-arrow" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>

            <!-- Price Filter -->
            <div class="filter-item">
              <select v-model="selectedPrice" class="filter-select">
                <option value="">ราคา</option>
                <option v-for="price in priceRanges" :key="price.value" :value="price.value">
                  {{ price.label }}
                </option>
              </select>
              <svg class="select-arrow" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>

            <!-- More Filters Button -->
            <button class="more-filters-btn" @click="showAdvancedFilters = true">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <line x1="4" y1="21" x2="4" y2="14"/>
                <line x1="4" y1="10" x2="4" y2="3"/>
                <line x1="12" y1="21" x2="12" y2="12"/>
                <line x1="12" y1="8" x2="12" y2="3"/>
                <line x1="20" y1="21" x2="20" y2="16"/>
                <line x1="20" y1="12" x2="20" y2="3"/>
                <line x1="1" y1="14" x2="7" y2="14"/>
                <line x1="9" y1="8" x2="15" y2="8"/>
                <line x1="17" y1="16" x2="23" y2="16"/>
              </svg>
              Filters
            </button>
          </div>
        </div>

        <!-- Results Info -->
        <div class="results-info">
          <div class="results-count">
            <span class="count-label">พบข้อมูล</span>
            <span class="count-number">{{ totalResults.toLocaleString() }}</span>
            <span class="count-text">ประกาศ</span>
          </div>
          <div class="sort-wrapper">
            <span class="sort-label">เรียงตาม:</span>
            <select v-model="currentSort" class="sort-select">
              <option v-for="sort in sortOptions" :key="sort.value" :value="sort.value">
                {{ sort.label }}
              </option>
            </select>
          </div>
        </div>
      </div>
    </section>

    <!-- Properties Listing -->
    <section class="properties-section">
      <div class="container">
        <div class="properties-list">
          <article 
            v-for="property in properties" 
            :key="property.id"
            class="property-card"
          >
            <!-- Property Image -->
            <div class="property-image">
              <img :src="property.image" :alt="property.title" />
              <div v-if="property.isPremium" class="premium-badge">Premium</div>
              <div class="property-type-badge">{{ property.propertyType }}</div>
              <button class="favorite-btn" @click.prevent="toggleFavorite(property.id)">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/>
                </svg>
              </button>
            </div>

            <!-- Property Content -->
            <div class="property-content">
              <div class="property-header">
                <h3 class="property-title">{{ property.title }}</h3>
                <div class="property-price">
                  <span class="price-currency">฿</span>
                  <span class="price-amount">{{ formatPrice(property.price) }}</span>
                </div>
              </div>

              <div class="property-location">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
                  <circle cx="12" cy="10" r="3"/>
                </svg>
                <NuxtLink :to="property.locationLink" class="location-link">{{ property.location }}</NuxtLink>
                <span class="location-detail">{{ property.district }}</span>
              </div>

              <div class="property-specs">
                <div class="spec-item" v-if="property.bedrooms">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M2 4v16M2 8h18a2 2 0 0 1 2 2v10M2 17h20M6 8v9"/>
                  </svg>
                  <span>{{ property.bedrooms }} นอน</span>
                </div>
                <div class="spec-item" v-if="property.bathrooms">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M9 6 6.5 3.5a1.5 1.5 0 0 0-1-.5C4.683 3 4 3.683 4 4.5V17a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2v-5"/>
                  </svg>
                  <span>{{ property.bathrooms }} น้ำ</span>
                </div>
                <div class="spec-item" v-if="property.floors">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <rect x="4" y="2" width="16" height="20" rx="2" ry="2"/>
                    <line x1="4" y1="10" x2="20" y2="10"/>
                  </svg>
                  <span>{{ property.floors }} ชั้น</span>
                </div>
                <div class="spec-item" v-if="property.landArea">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <rect width="18" height="18" x="3" y="3" rx="2" ry="2"/>
                  </svg>
                  <span>{{ property.landArea }} ตร.ว.</span>
                </div>
                <div class="spec-item" v-if="property.usableArea">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"/>
                  </svg>
                  <span>{{ property.usableArea }} ตร.ม.</span>
                </div>
              </div>

              <div class="property-footer">
                <div class="posted-date">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <circle cx="12" cy="12" r="10"/>
                    <polyline points="12 6 12 12 16 14"/>
                  </svg>
                  <span>เลื่อนประกาศ: {{ property.postedDate }}</span>
                </div>
                <NuxtLink :to="`/property/${property.id}`" class="view-btn">
                  ดูรายละเอียด
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M5 12h14M12 5l7 7-7 7"/>
                  </svg>
                </NuxtLink>
              </div>
            </div>
          </article>
        </div>

        <!-- Pagination -->
        <div class="pagination">
          <button class="page-btn prev" :disabled="currentPage === 1" @click="currentPage--">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polyline points="15 18 9 12 15 6"/>
            </svg>
          </button>
          <button 
            v-for="page in visiblePages" 
            :key="page"
            :class="['page-btn', { active: currentPage === page }]"
            @click="currentPage = page"
          >
            {{ page }}
          </button>
          <button class="page-btn next" :disabled="currentPage === totalPages" @click="currentPage++">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polyline points="9 18 15 12 9 6"/>
            </svg>
          </button>
        </div>
      </div>
    </section>

    <Footer />
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';

// Filter States
const activeDropdown = ref(null);
const selectedZone = ref('');
const selectedListingType = ref('');
const selectedRooms = ref('');
const selectedPrice = ref('');
const showAdvancedFilters = ref(false);
const currentSort = ref('latest');
const currentPage = ref(1);
const totalResults = ref(2982);
const totalPages = ref(100);

// Filter Options
const zones = [
  { value: 'all', label: 'ทุกโซน' },
  { value: 'sukhumvit', label: 'สุขุมวิท' },
  { value: 'silom', label: 'สีลม/สาทร' },
  { value: 'ratchada', label: 'รัชดา/พระราม 9' },
  { value: 'ladprao', label: 'ลาดพร้าว/รามอินทรา' },
  { value: 'bangna', label: 'บางนา/ศรีนครินทร์' },
  { value: 'thonburi', label: 'ธนบุรี/เพชรเกษม' },
  { value: 'onnut', label: 'อ่อนนุช/พระโขนง' },
  { value: 'rama3', label: 'พระราม 3' },
  { value: 'phayathai', label: 'พญาไท/อารีย์' },
];

const listingTypes = [
  { value: 'all', label: 'ทุกประเภท' },
  { value: 'sale', label: 'ขาย' },
  { value: 'rent', label: 'เช่า' },
  { value: 'sale-rent', label: 'ขาย/เช่า' },
];

const roomOptions = [
  { value: 'all', label: 'ทุกขนาด' },
  { value: 'studio', label: 'สตูดิโอ' },
  { value: '1', label: '1 ห้องนอน' },
  { value: '2', label: '2 ห้องนอน' },
  { value: '3', label: '3 ห้องนอน' },
  { value: '4+', label: '4+ ห้องนอน' },
];

const priceRanges = [
  { value: 'all', label: 'ทุกราคา' },
  { value: '0-1m', label: 'ไม่เกิน 1 ล้าน' },
  { value: '1-3m', label: '1 - 3 ล้าน' },
  { value: '3-5m', label: '3 - 5 ล้าน' },
  { value: '5-10m', label: '5 - 10 ล้าน' },
  { value: '10-20m', label: '10 - 20 ล้าน' },
  { value: '20m+', label: '20 ล้านขึ้นไป' },
];

const sortOptions = [
  { value: 'latest', label: 'เลื่อนประกาศล่าสุด' },
  { value: 'price-low', label: 'ราคาต่ำ-สูง' },
  { value: 'price-high', label: 'ราคาสูง-ต่ำ' },
  { value: 'area', label: 'พื้นที่' },
];

// Sample Properties Data
const properties = ref([
  {
    id: 1,
    title: 'ขาย บ้านแฝด อุดมทรัพย์ 160 ตรม หลังมุม 41.4 ตรว น่าอยู่',
    propertyType: 'บ้านแฝด',
    location: 'Udom Sap (Bang Srimueang)',
    locationLink: '/location/udom-sap',
    district: 'เมืองนนทบุรี นนทบุรี',
    price: 3300000,
    bedrooms: 3,
    bathrooms: 2,
    floors: 2,
    landArea: 41,
    usableArea: 160,
    image: 'https://images.unsplash.com/photo-1600596542815-ffad4c1539a9?w=600',
    isPremium: true,
    postedDate: '22/11/2025 20:08',
  },
  {
    id: 2,
    title: '🔥🔥ขายบ้านเดี่ยว🔥🔥 โครงการหมู่บ้านรัชชา7 พระราม5 (ไม่รับนาย...)',
    propertyType: 'บ้านเดี่ยว',
    location: 'Baan Ratcha 7',
    locationLink: '/location/baan-ratcha',
    district: 'บางกรวย นนทบุรี',
    price: 4800000,
    bedrooms: 3,
    bathrooms: 3,
    floors: 2,
    landArea: 37,
    usableArea: 180,
    image: 'https://images.unsplash.com/photo-1600607687939-ce8a6c25118c?w=600',
    isPremium: true,
    postedDate: '23/01/2025 11:57',
  },
  {
    id: 3,
    title: 'คอนโด IDEO Q สุขุมวิท 36 ห้องสวย พร้อมอยู่',
    propertyType: 'คอนโด',
    location: 'IDEO Q Sukhumvit 36',
    locationLink: '/location/ideo-q-sukhumvit',
    district: 'คลองเตย กรุงเทพฯ',
    price: 5990000,
    bedrooms: 1,
    bathrooms: 1,
    floors: null,
    landArea: null,
    usableArea: 35,
    image: 'https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=600',
    isPremium: false,
    postedDate: '20/11/2025 15:30',
  },
  {
    id: 4,
    title: 'The Line พหลโยธิน คอนโดหรูใกล้ BTS',
    propertyType: 'คอนโด',
    location: 'The Line Phahol-Pradipat',
    locationLink: '/location/the-line',
    district: 'จตุจักร กรุงเทพฯ',
    price: 7500000,
    bedrooms: 2,
    bathrooms: 2,
    floors: null,
    landArea: null,
    usableArea: 55,
    image: 'https://images.unsplash.com/photo-1560448204-e02f11c3d0e2?w=600',
    isPremium: true,
    postedDate: '21/11/2025 09:45',
  },
]);

// Computed
const visiblePages = computed(() => {
  const pages = [];
  const total = totalPages.value;
  const current = currentPage.value;
  
  let start = Math.max(1, current - 2);
  let end = Math.min(total, current + 2);
  
  if (current <= 3) {
    end = Math.min(5, total);
  }
  if (current >= total - 2) {
    start = Math.max(1, total - 4);
  }
  
  for (let i = start; i <= end; i++) {
    pages.push(i);
  }
  return pages;
});

// Methods
const toggleDropdown = (dropdown) => {
  activeDropdown.value = activeDropdown.value === dropdown ? null : dropdown;
};

const selectZone = (zone) => {
  selectedZone.value = zone.label;
  activeDropdown.value = null;
};

const selectListingType = (type) => {
  selectedListingType.value = type.label;
  activeDropdown.value = null;
};

const selectRooms = (room) => {
  selectedRooms.value = room.label;
  activeDropdown.value = null;
};

const selectPrice = (price) => {
  selectedPrice.value = price.label;
  activeDropdown.value = null;
};

const performSearch = () => {
  console.log('Search with filters:', {
    zone: selectedZone.value,
    listingType: selectedListingType.value,
    rooms: selectedRooms.value,
    price: selectedPrice.value,
  });
};

const formatPrice = (price) => {
  return new Intl.NumberFormat('th-TH').format(price);
};

const toggleFavorite = (id) => {
  console.log('Toggle favorite:', id);
};

// Close dropdown when clicking outside
const closeDropdowns = (e) => {
  if (!e.target.closest('.filter-dropdown')) {
    activeDropdown.value = null;
  }
};

onMounted(() => {
  document.addEventListener('click', closeDropdowns);
});

onUnmounted(() => {
  document.removeEventListener('click', closeDropdowns);
});
</script>

<style scoped>
.condo-page {
  min-height: 100vh;
  background: linear-gradient(180deg, #fffef7 0%, #f8f4e8 100%);
}

/* Page Header */
.page-header {
  background: linear-gradient(135deg, #1a1a1a 0%, #2c2c2c 100%);
  padding: 60px 0 40px;
  text-align: center;
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 20px;
}

.page-title {
  font-size: 2.5rem;
  font-weight: 700;
  color: #DAA520;
  margin: 0 0 10px 0;
}

.page-subtitle {
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.7);
  margin: 0;
}

/* Filter Section */
.filter-section {
  background: linear-gradient(180deg, #f8f6f0 0%, #fff 100%);
  padding: 20px 0;
  position: sticky;
  top: 70px;
  z-index: 100;
}

.filter-card {
  background: #fff;
  border-radius: 12px;
  padding: 15px 20px;
  box-shadow: 0 2px 15px rgba(0, 0, 0, 0.08);
  border: 1px solid rgba(212, 175, 55, 0.15);
}

.filter-row {
  display: flex;
  align-items: center;
  gap: 12px;
  flex-wrap: wrap;
}

.filter-item {
  position: relative;
  flex: 1;
  min-width: 150px;
}

.filter-select {
  width: 100%;
  padding: 12px 40px 12px 15px;
  border: 2px solid #e5e5e5;
  border-radius: 8px;
  font-size: 0.9rem;
  color: #2c2c2c;
  background: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
}

.filter-select:hover {
  border-color: #DAA520;
}

.filter-select:focus {
  outline: none;
  border-color: #DAA520;
  box-shadow: 0 0 0 3px rgba(212, 175, 55, 0.1);
}

.select-arrow {
  position: absolute;
  right: 12px;
  top: 50%;
  transform: translateY(-50%);
  width: 16px;
  height: 16px;
  color: #DAA520;
  pointer-events: none;
}

.more-filters-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 18px;
  background: #fff;
  border: 2px dashed rgba(212, 175, 55, 0.4);
  border-radius: 8px;
  font-size: 0.9rem;
  color: #666;
  cursor: pointer;
  transition: all 0.3s ease;
  white-space: nowrap;
}

.more-filters-btn:hover {
  border-color: #DAA520;
  color: #DAA520;
  background: rgba(212, 175, 55, 0.05);
}

.more-filters-btn svg {
  width: 18px;
  height: 18px;
}

/* Results Info */
.results-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
  padding: 15px 0 0;
}

.results-count {
  display: flex;
  align-items: baseline;
  gap: 6px;
}

.count-label {
  font-size: 0.95rem;
  color: #666;
}

.count-number {
  font-size: 1.3rem;
  font-weight: 700;
  color: #DAA520;
}

.count-text {
  font-size: 0.95rem;
  color: #666;
}

.sort-wrapper {
  display: flex;
  align-items: center;
  gap: 10px;
}

.sort-label {
  font-size: 0.9rem;
  color: #666;
}

.sort-select {
  padding: 8px 30px 8px 12px;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 0.9rem;
  color: #2c2c2c;
  background: #fff;
  cursor: pointer;
  appearance: none;
  -webkit-appearance: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 24 24' fill='none' stroke='%23DAA520' stroke-width='2'%3E%3Cpolyline points='6 9 12 15 18 9'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 10px center;
}

.sort-select:focus {
  outline: none;
  border-color: #DAA520;
}

/* Properties Section */
.properties-section {
  padding: 40px 0 60px;
}

.properties-list {
  display: flex;
  flex-direction: column;
  gap: 25px;
}

/* Property Card */
.property-card {
  display: flex;
  background: #fff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 5px 20px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
  border: 1px solid rgba(212, 175, 55, 0.1);
}

.property-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 40px rgba(212, 175, 55, 0.15);
  border-color: rgba(212, 175, 55, 0.3);
}

.property-image {
  position: relative;
  width: 350px;
  min-height: 220px;
  flex-shrink: 0;
}

.property-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.premium-badge {
  position: absolute;
  top: 15px;
  left: 15px;
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  color: #fff;
  padding: 6px 14px;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  box-shadow: 0 4px 12px rgba(212, 175, 55, 0.4);
}

.property-type-badge {
  position: absolute;
  bottom: 15px;
  left: 15px;
  background: rgba(0, 0, 0, 0.7);
  color: #fff;
  padding: 5px 12px;
  border-radius: 4px;
  font-size: 0.75rem;
  font-weight: 500;
}

.favorite-btn {
  position: absolute;
  top: 15px;
  right: 15px;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.9);
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.favorite-btn svg {
  width: 20px;
  height: 20px;
  color: #999;
}

.favorite-btn:hover {
  background: #ef4444;
}

.favorite-btn:hover svg {
  color: #fff;
  fill: #fff;
}

.property-content {
  flex: 1;
  padding: 25px;
  display: flex;
  flex-direction: column;
}

.property-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 12px;
}

.property-title {
  font-size: 1.15rem;
  font-weight: 600;
  color: #1a1a1a;
  margin: 0;
  flex: 1;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.property-price {
  display: flex;
  align-items: baseline;
  gap: 2px;
  flex-shrink: 0;
  margin-left: 20px;
}

.price-currency {
  font-size: 1rem;
  font-weight: 600;
  color: #DAA520;
}

.price-amount {
  font-size: 1.5rem;
  font-weight: 700;
  color: #DAA520;
}

.property-location {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 15px;
  flex-wrap: wrap;
}

.property-location svg {
  width: 16px;
  height: 16px;
  color: #DAA520;
}

.location-link {
  color: #3b82f6;
  text-decoration: underline;
  font-size: 0.9rem;
}

.location-link:hover {
  color: #2563eb;
}

.location-detail {
  font-size: 0.9rem;
  color: #666;
}

.property-specs {
  display: flex;
  gap: 20px;
  padding: 15px 0;
  border-top: 1px solid #eee;
  border-bottom: 1px solid #eee;
  margin-bottom: 15px;
  flex-wrap: wrap;
}

.spec-item {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.85rem;
  color: #666;
}

.spec-item svg {
  width: 16px;
  height: 16px;
  color: #999;
}

.property-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: auto;
}

.posted-date {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.8rem;
  color: #999;
}

.posted-date svg {
  width: 14px;
  height: 14px;
}

.view-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 20px;
  background: linear-gradient(135deg, #1a1a1a 0%, #333 100%);
  color: #fff;
  text-decoration: none;
  border-radius: 8px;
  font-size: 0.85rem;
  font-weight: 500;
  transition: all 0.3s ease;
}

.view-btn svg {
  width: 16px;
  height: 16px;
  transition: transform 0.3s ease;
}

.view-btn:hover {
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
}

.view-btn:hover svg {
  transform: translateX(5px);
}

/* Pagination */
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
  margin-top: 50px;
}

.page-btn {
  width: 44px;
  height: 44px;
  border-radius: 10px;
  background: #fff;
  border: 2px solid rgba(212, 175, 55, 0.3);
  color: #2c2c2c;
  font-size: 0.95rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.page-btn:hover:not(:disabled) {
  border-color: #DAA520;
  color: #DAA520;
}

.page-btn.active {
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  border-color: #DAA520;
  color: #fff;
}

.page-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.page-btn svg {
  width: 18px;
  height: 18px;
}

/* Responsive */
@media (max-width: 1199px) {
  .property-image {
    width: 300px;
  }
}

@media (max-width: 991px) {
  .filter-bar {
    flex-wrap: wrap;
  }
  
  .filter-btn {
    min-width: calc(50% - 6px);
  }
  
  .search-btn {
    width: 100%;
    justify-content: center;
    margin-top: 10px;
  }
  
  .property-card {
    flex-direction: column;
  }
  
  .property-image {
    width: 100%;
    height: 250px;
  }
  
  .results-info {
    flex-direction: column;
    gap: 15px;
    align-items: flex-start;
  }
  
  .sort-options {
    flex-wrap: wrap;
  }
}

@media (max-width: 767px) {
  .page-header {
    padding: 40px 0 30px;
  }
  
  .page-title {
    font-size: 1.8rem;
  }
  
  .filter-section {
    position: static;
  }
  
  .filter-btn {
    min-width: 100%;
    flex: 1;
  }
  
  .property-header {
    flex-direction: column;
    gap: 10px;
  }
  
  .property-price {
    margin-left: 0;
  }
  
  .property-specs {
    gap: 12px;
  }
  
  .property-footer {
    flex-direction: column;
    gap: 15px;
    align-items: flex-start;
  }
  
  .view-btn {
    width: 100%;
    justify-content: center;
  }
}

@media (max-width: 575px) {
  .page-title {
    font-size: 1.5rem;
  }
  
  .page-subtitle {
    font-size: 0.95rem;
  }
  
  .pagination {
    gap: 5px;
  }
  
  .page-btn {
    width: 38px;
    height: 38px;
    font-size: 0.85rem;
  }
}
</style>

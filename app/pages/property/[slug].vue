<template>
  <div class="property-detail-page">
    <Navbar />
    
    <!-- Property Header -->
    <section class="property-header">
      <div class="container">
        <div class="header-top">
          <div class="price-badge">
            <span class="price-label">ขาย</span>
            <span class="price-amount">฿ {{ formatPrice(property.price) }}</span>
          </div>
          <div class="header-specs">
            <div class="spec-item">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <rect x="3" y="3" width="18" height="18" rx="2" ry="2"/>
              </svg>
              <span>{{ property.size }} ตร.ม.</span>
            </div>
            <div class="spec-item">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M2 4v16M2 8h18a2 2 0 0 1 2 2v10M2 17h20M6 8v9"/>
              </svg>
              <span>{{ property.bedrooms }} ห้องนอน</span>
            </div>
            <div class="spec-item">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M9 6 6.5 3.5a1.5 1.5 0 0 0-1-.5C4.683 3 4 3.683 4 4.5V17a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2v-5"/>
              </svg>
              <span>{{ property.bathrooms }} ห้องน้ำ</span>
            </div>
            <div class="spec-item">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <rect x="4" y="2" width="16" height="20" rx="2" ry="2"/>
                <line x1="4" y1="10" x2="20" y2="10"/>
              </svg>
              <span>ชั้น {{ property.floor }}</span>
            </div>
          </div>
          <div class="header-actions">
            <button class="action-btn share">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <circle cx="18" cy="5" r="3"/>
                <circle cx="6" cy="12" r="3"/>
                <circle cx="18" cy="19" r="3"/>
                <line x1="8.59" y1="13.51" x2="15.42" y2="17.49"/>
                <line x1="15.41" y1="6.51" x2="8.59" y2="10.49"/>
              </svg>
            </button>
            <button class="action-btn favorite">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/>
              </svg>
            </button>
            <button class="contact-btn">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72"/>
              </svg>
              ติดต่อสอบถาม
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- Main Content -->
    <section class="main-content">
      <div class="container">
        <div class="content-grid">
          <!-- Left Column - Gallery & Details -->
          <div class="content-left">
            <!-- Image Gallery -->
            <div class="gallery-section">
              <div class="main-image" @click="openImageModal">
                <img :src="currentImage" :alt="property.title" />
                <div class="image-counter">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <rect x="3" y="3" width="18" height="18" rx="2" ry="2"/>
                    <circle cx="8.5" cy="8.5" r="1.5"/>
                    <polyline points="21 15 16 10 5 21"/>
                  </svg>
                  {{ property.images.length }} รูปภาพ
                </div>
                <button class="gallery-nav prev" @click.stop="prevImage">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <polyline points="15 18 9 12 15 6"/>
                  </svg>
                </button>
                <button class="gallery-nav next" @click.stop="nextImage">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <polyline points="9 18 15 12 9 6"/>
                  </svg>
                </button>
              </div>
              <div class="thumbnail-list">
                <button 
                  v-for="(image, index) in property.images" 
                  :key="index"
                  :class="['thumbnail', { active: currentImageIndex === index }]"
                  @click="currentImageIndex = index"
                >
                  <img :src="image" :alt="`รูปที่ ${index + 1}`" />
                </button>
              </div>
            </div>

    <!-- Image Modal -->
    <Teleport to="body">
      <div v-if="isModalOpen" class="image-modal" @click="closeImageModal">
        <div class="modal-header">
          <div class="modal-counter">{{ currentImageIndex + 1 }} / {{ property.images.length }}</div>
          <div class="modal-actions">
            <button class="modal-action-btn" @click.stop title="ดาวน์โหลด">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/>
                <polyline points="7 10 12 15 17 10"/>
                <line x1="12" y1="15" x2="12" y2="3"/>
              </svg>
            </button>
            <button class="modal-action-btn close" @click.stop="closeImageModal" title="ปิด">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <line x1="18" y1="6" x2="6" y2="18"/>
                <line x1="6" y1="6" x2="18" y2="18"/>
              </svg>
            </button>
          </div>
        </div>
        
        <div class="modal-content" @click.stop>
          <button class="modal-nav prev" @click.stop="prevImage">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polyline points="15 18 9 12 15 6"/>
            </svg>
          </button>
          
          <div class="modal-image-wrapper">
            <img :src="currentImage" :alt="property.title" />
          </div>
          
          <button class="modal-nav next" @click.stop="nextImage">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polyline points="9 18 15 12 9 6"/>
            </svg>
          </button>
        </div>
        
        <div class="modal-thumbnails" @click.stop>
          <button 
            v-for="(image, index) in property.images" 
            :key="index"
            :class="['modal-thumb', { active: currentImageIndex === index }]"
            @click="currentImageIndex = index"
          >
            <img :src="image" :alt="`รูปที่ ${index + 1}`" />
          </button>
        </div>
      </div>
    </Teleport>

            <!-- Property Info Card -->
            <div class="info-card">
              <div class="card-header">
                <span class="property-id">รหัสทรัพย์: {{ property.id }}</span>
                <span class="post-date">โพสต์เมื่อ: {{ property.postedDate }}</span>
              </div>
              <h1 class="property-title">
                <span class="listing-type">{{ property.listingType }}</span>
                {{ property.title }}
              </h1>
              <div class="property-location">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
                  <circle cx="12" cy="10" r="3"/>
                </svg>
                <span>{{ property.location }}</span>
              </div>
              <div class="property-price-full">
                <span class="price-amount">฿ {{ formatPrice(property.price) }}</span>
                <span class="price-per-sqm">({{ formatPrice(Math.round(property.price / property.size)) }} บาท/ตร.ม.)</span>
              </div>
            </div>

            <!-- Specifications -->
            <div class="specs-card">
              <h2 class="section-title">ข้อมูลสรุป</h2>
              <div class="specs-grid">
                <div class="spec-box">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <rect x="3" y="3" width="18" height="18" rx="2" ry="2"/>
                  </svg>
                  <div class="spec-info">
                    <span class="spec-value">{{ property.size }}</span>
                    <span class="spec-label">ตร.ม.</span>
                  </div>
                </div>
                <div class="spec-box">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M2 4v16M2 8h18a2 2 0 0 1 2 2v10M2 17h20M6 8v9"/>
                  </svg>
                  <div class="spec-info">
                    <span class="spec-value">{{ property.bedrooms }}</span>
                    <span class="spec-label">ห้องนอน</span>
                  </div>
                </div>
                <div class="spec-box">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M9 6 6.5 3.5a1.5 1.5 0 0 0-1-.5C4.683 3 4 3.683 4 4.5V17a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2v-5"/>
                  </svg>
                  <div class="spec-info">
                    <span class="spec-value">{{ property.bathrooms }}</span>
                    <span class="spec-label">ห้องน้ำ</span>
                  </div>
                </div>
                <div class="spec-box">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <rect x="4" y="2" width="16" height="20" rx="2" ry="2"/>
                    <line x1="4" y1="10" x2="20" y2="10"/>
                  </svg>
                  <div class="spec-info">
                    <span class="spec-value">{{ property.floor }}</span>
                    <span class="spec-label">ชั้น</span>
                  </div>
                </div>
              </div>
            </div>

            <!-- Description -->
            <div class="description-card">
              <h2 class="section-title">รายละเอียด</h2>
              <div class="project-info">
                <div class="info-row">
                  <span class="info-label">ชื่อโครงการ</span>
                  <NuxtLink to="#" class="info-value link">{{ property.projectName }}</NuxtLink>
                </div>
              </div>
              <div class="description-content">
                <p>{{ property.description }}</p>
              </div>
            </div>

            <!-- Amenities -->
            <div class="amenities-card">
              <h2 class="section-title">สิ่งที่มีให้</h2>
              <div class="amenities-grid">
                <div v-for="amenity in property.amenities" :key="amenity" class="amenity-item">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <polyline points="20 6 9 17 4 12"/>
                  </svg>
                  <span>{{ amenity }}</span>
                </div>
              </div>
            </div>

            <!-- Facilities -->
            <div class="facilities-card">
              <h2 class="section-title">สิ่งอำนวยความสะดวก</h2>
              <div class="facilities-grid">
                <div v-for="facility in property.facilities" :key="facility" class="facility-item">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <polyline points="20 6 9 17 4 12"/>
                  </svg>
                  <span>{{ facility }}</span>
                </div>
              </div>
            </div>

            <!-- Nearby Places -->
            <div class="nearby-card">
              <h2 class="section-title">สถานที่ใกล้เคียง</h2>
              <div class="nearby-tabs">
                <button 
                  v-for="tab in nearbyTabs"
                  :key="tab.value"
                  :class="['tab-btn', { active: activeNearbyTab === tab.value }]"
                  @click="activeNearbyTab = tab.value"
                >
                  {{ tab.label }}
                </button>
              </div>
              <table class="nearby-table">
                <thead>
                  <tr>
                    <th>สถานที่</th>
                    <th>ระยะทาง</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="place in filteredNearbyPlaces" :key="place.name">
                    <td>{{ place.name }}</td>
                    <td>{{ place.distance }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>

          <!-- Right Column - Agent & Contact -->
          <div class="content-right">
            <!-- Agent Card -->
            <div class="agent-card">
              <div class="agent-header">
                <span>ลงประกาศโดย</span>
              </div>
              <div class="inquiry-tabs">
                <button :class="['inquiry-tab', { active: inquiryType === 'message' }]" @click="inquiryType = 'message'">
                  ส่งข้อความ
                </button>
                <button :class="['inquiry-tab', { active: inquiryType === 'phone' }]" @click="inquiryType = 'phone'">
                  นัดหมายโทรกลับ
                </button>
              </div>
              
              <div class="agent-info">
                <img :src="property.agent.avatar" :alt="property.agent.name" class="agent-avatar" />
                <div class="agent-details">
                  <h3 class="agent-name">{{ property.agent.name }}</h3>
                  <p class="agent-company">{{ property.agent.company }}</p>
                </div>
              </div>

              <div class="contact-form">
                <input type="text" v-model="contactForm.name" placeholder="ชื่อ-นามสกุล" class="form-input" />
                <input type="tel" v-model="contactForm.phone" placeholder="เบอร์โทรศัพท์" class="form-input" />
                <textarea v-model="contactForm.message" placeholder="ข้อความ..." class="form-textarea" rows="3"></textarea>
                <button class="submit-btn">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72"/>
                  </svg>
                  ส่งข้อความ / ขอนัดชมทรัพย์
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

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

<style scoped>
.property-detail-page {
  min-height: 100vh;
  background: #f8f6f0;
  overflow-x: hidden;
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 20px;
  overflow-x: hidden;
}

/* Property Header */
.property-header {
  background: #fff;
  padding: 15px 0;
  border-bottom: 1px solid #eee;
  position: sticky;
  top: 70px;
  z-index: 100;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
}

.header-top {
  display: flex;
  align-items: center;
  gap: 30px;
  flex-wrap: wrap;
}

.price-badge {
  display: flex;
  align-items: center;
  gap: 10px;
}

.price-label {
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  color: #fff;
  padding: 4px 12px;
  border-radius: 4px;
  font-size: 0.8rem;
  font-weight: 600;
}

.price-amount {
  font-size: 1.5rem;
  font-weight: 700;
  color: #DAA520;
}

.header-specs {
  display: flex;
  gap: 20px;
  flex: 1;
}

.header-specs .spec-item {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.9rem;
  color: #666;
}

.header-specs .spec-item svg {
  width: 18px;
  height: 18px;
  color: #999;
}

.header-actions {
  display: flex;
  align-items: center;
  gap: 10px;
}

.action-btn {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: #f5f5f5;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.action-btn svg {
  width: 20px;
  height: 20px;
  color: #666;
}

.action-btn:hover {
  background: #DAA520;
}

.action-btn:hover svg {
  color: #fff;
}

.contact-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 24px;
  background: linear-gradient(135deg, #10b981 0%, #059669 100%);
  color: #fff;
  border: none;
  border-radius: 8px;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.contact-btn svg {
  width: 18px;
  height: 18px;
}

.contact-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(16, 185, 129, 0.4);
}

/* Main Content Grid */
.main-content {
  padding: 30px 0 60px;
  overflow-x: hidden;
}

.content-grid {
  display: grid;
  grid-template-columns: 1fr 380px;
  gap: 30px;
  max-width: 100%;
  overflow-x: hidden;
}

.content-left {
  display: flex;
  flex-direction: column;
  gap: 25px;
  min-width: 0;
  max-width: 100%;
}

/* Gallery */
.gallery-section {
  background: #fff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 2px 15px rgba(0, 0, 0, 0.08);
  max-width: 100%;
}

.main-image {
  position: relative;
  aspect-ratio: 16/9;
  max-width: 100%;
  overflow: hidden;
}

.main-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  max-width: 100%;
}

.image-counter {
  position: absolute;
  bottom: 15px;
  left: 15px;
  background: rgba(0, 0, 0, 0.6);
  color: #fff;
  padding: 5px 12px;
  border-radius: 6px;
  font-size: 0.85rem;
}

.gallery-nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.9);
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.gallery-nav svg {
  width: 20px;
  height: 20px;
  color: #333;
}

.gallery-nav:hover {
  background: #DAA520;
}

.gallery-nav:hover svg {
  color: #fff;
}

.gallery-nav.prev {
  left: 15px;
}

.gallery-nav.next {
  right: 15px;
}

.thumbnail-list {
  display: flex;
  gap: 10px;
  padding: 15px;
  overflow-x: auto;
}

.thumbnail {
  width: 80px;
  height: 60px;
  border-radius: 8px;
  overflow: hidden;
  border: 2px solid transparent;
  cursor: pointer;
  transition: all 0.3s ease;
  flex-shrink: 0;
}

.thumbnail img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.thumbnail.active {
  border-color: #DAA520;
}

/* Info Cards */
.info-card,
.specs-card,
.description-card,
.amenities-card,
.facilities-card,
.nearby-card {
  background: #fff;
  border-radius: 16px;
  padding: 25px;
  box-shadow: 0 2px 15px rgba(0, 0, 0, 0.08);
}

.card-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 15px;
  font-size: 0.85rem;
  color: #999;
}

.property-title {
  font-size: 1.3rem;
  font-weight: 600;
  color: #1a1a1a;
  margin: 0 0 12px 0;
  line-height: 1.4;
}

.listing-type {
  color: #3b82f6;
  font-weight: 700;
}

.property-location {
  display: flex;
  align-items: center;
  gap: 8px;
  color: #666;
  margin-bottom: 15px;
}

.property-location svg {
  width: 18px;
  height: 18px;
  color: #DAA520;
}

.property-price-full {
  display: flex;
  align-items: baseline;
  gap: 10px;
}

.property-price-full .price-amount {
  font-size: 1.8rem;
  font-weight: 700;
  color: #DAA520;
}

.price-per-sqm {
  font-size: 0.9rem;
  color: #999;
}

/* Section Title */
.section-title {
  font-size: 1.1rem;
  font-weight: 600;
  color: #1a1a1a;
  margin: 0 0 20px 0;
  padding-bottom: 10px;
  border-bottom: 2px solid #DAA520;
  display: inline-block;
}

/* Specs Grid */
.specs-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

.spec-box {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 15px;
  background: #f8f6f0;
  border-radius: 12px;
}

.spec-box svg {
  width: 32px;
  height: 32px;
  color: #DAA520;
}

.spec-info {
  display: flex;
  flex-direction: column;
}

.spec-value {
  font-size: 1.3rem;
  font-weight: 700;
  color: #1a1a1a;
}

.spec-label {
  font-size: 0.85rem;
  color: #666;
}

/* Description */
.project-info {
  margin-bottom: 20px;
}

.info-row {
  display: flex;
  gap: 15px;
  padding: 10px 0;
  border-bottom: 1px solid #eee;
}

.info-label {
  color: #666;
  min-width: 120px;
}

.info-value.link {
  color: #3b82f6;
  text-decoration: none;
}

.info-value.link:hover {
  text-decoration: underline;
}

.description-content p {
  color: #666;
  line-height: 1.7;
  margin: 0;
}

/* Amenities & Facilities */
.amenities-grid,
.facilities-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 15px;
}

.amenity-item,
.facility-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px;
  background: #f8f6f0;
  border-radius: 8px;
  font-size: 0.9rem;
  color: #333;
}

.amenity-item svg,
.facility-item svg {
  width: 18px;
  height: 18px;
  color: #10b981;
}

/* Nearby Places */
.nearby-tabs {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
  overflow-x: auto;
  padding-bottom: 5px;
}

.tab-btn {
  padding: 8px 16px;
  background: #f5f5f5;
  border: none;
  border-radius: 6px;
  font-size: 0.85rem;
  color: #666;
  cursor: pointer;
  transition: all 0.3s ease;
  white-space: nowrap;
}

.tab-btn:hover {
  background: #eee;
}

.tab-btn.active {
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  color: #fff;
}

.nearby-table {
  width: 100%;
  border-collapse: collapse;
}

.nearby-table th,
.nearby-table td {
  padding: 12px 15px;
  text-align: left;
  border-bottom: 1px solid #eee;
}

.nearby-table th {
  background: #f8f6f0;
  font-weight: 600;
  color: #333;
}

.nearby-table td {
  color: #666;
}

/* Right Column */
.content-right {
  display: flex;
  flex-direction: column;
  gap: 25px;
}

/* Agent Card */
.agent-card {
  background: #fff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 2px 15px rgba(0, 0, 0, 0.08);
  position: sticky;
  top: 140px;
}

.agent-header {
  padding: 15px 20px;
  background: #f8f6f0;
  font-size: 0.9rem;
  color: #666;
}

.inquiry-tabs {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.inquiry-tab {
  padding: 12px;
  background: #fff;
  border: none;
  border-bottom: 2px solid #eee;
  font-size: 0.9rem;
  color: #666;
  cursor: pointer;
  transition: all 0.3s ease;
}

.inquiry-tab.active {
  color: #DAA520;
  border-bottom-color: #DAA520;
  font-weight: 600;
}

.agent-info {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 20px;
  border-bottom: 1px solid #eee;
}

.agent-avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  object-fit: cover;
  border: 2px solid #DAA520;
}

.agent-name {
  font-size: 1rem;
  font-weight: 600;
  color: #1a1a1a;
  margin: 0 0 4px 0;
}

.agent-company {
  font-size: 0.85rem;
  color: #666;
  margin: 0;
}

.contact-form {
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.form-input,
.form-textarea {
  width: 100%;
  padding: 12px 15px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 0.9rem;
  transition: all 0.3s ease;
}

.form-input:focus,
.form-textarea:focus {
  outline: none;
  border-color: #DAA520;
  box-shadow: 0 0 0 3px rgba(212, 175, 55, 0.1);
}

.form-textarea {
  resize: vertical;
  min-height: 80px;
}

.submit-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  padding: 14px;
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  color: #fff;
  border: none;
  border-radius: 8px;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.submit-btn svg {
  width: 20px;
  height: 20px;
}

.submit-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(212, 175, 55, 0.4);
}

/* Promo Card */
.promo-card {
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 2px 15px rgba(0, 0, 0, 0.08);
  position: relative;
}

.promo-card img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.promo-content {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 20px;
  background: linear-gradient(to top, rgba(0,0,0,0.8), transparent);
  color: #fff;
}

.promo-badge {
  background: #DAA520;
  padding: 4px 10px;
  border-radius: 4px;
  font-size: 0.75rem;
  font-weight: 600;
}

.promo-content h3 {
  font-size: 1.1rem;
  margin: 10px 0 5px;
}

.promo-content p {
  font-size: 0.9rem;
  margin: 0;
  opacity: 0.9;
}

/* Responsive */
@media (max-width: 1199px) {
  .content-grid {
    grid-template-columns: 1fr 320px;
  }
  
  .specs-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 991px) {
  .content-grid {
    grid-template-columns: 1fr;
  }
  
  .content-right {
    order: -1;
  }
  
  .agent-card {
    position: static;
  }
  
  .header-specs {
    display: none;
  }
}

@media (max-width: 767px) {
  .property-header {
    position: static;
  }
  
  .header-top {
    flex-direction: column;
    align-items: flex-start;
    gap: 15px;
  }
  
  .header-actions {
    width: 100%;
    justify-content: space-between;
  }
  
  .contact-btn {
    flex: 1;
    justify-content: center;
  }
  
  .specs-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  
  .amenities-grid,
  .facilities-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 575px) {
  .specs-grid {
    grid-template-columns: 1fr 1fr;
    gap: 10px;
  }
  
  .spec-box {
    padding: 12px;
    gap: 10px;
  }
  
  .spec-box svg {
    width: 24px;
    height: 24px;
  }
  
  .spec-value {
    font-size: 1.1rem;
  }
  
  .amenities-grid,
  .facilities-grid {
    grid-template-columns: 1fr;
  }
  
  .property-price-full .price-amount {
    font-size: 1.4rem;
  }
  
  .price-badge {
    flex-direction: column;
    align-items: flex-start;
    gap: 5px;
  }
  
  .price-amount {
    font-size: 1.3rem;
  }
  
  .info-card,
  .specs-card,
  .description-card,
  .amenities-card,
  .facilities-card,
  .nearby-card {
    padding: 20px 15px;
  }
  
  .agent-card {
    border-radius: 12px;
  }
  
  .contact-form {
    padding: 15px;
  }
}

/* Image Modal Styles */
.image-modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.95);
  z-index: 10000;
  display: flex;
  flex-direction: column;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 20px;
  background: rgba(0, 0, 0, 0.5);
}

.modal-counter {
  color: #fff;
  font-size: 1rem;
  font-weight: 500;
}

.modal-actions {
  display: flex;
  gap: 10px;
}

.modal-action-btn {
  width: 40px;
  height: 40px;
  border-radius: 8px;
  background: rgba(255, 255, 255, 0.1);
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.modal-action-btn svg {
  width: 22px;
  height: 22px;
  color: #fff;
}

.modal-action-btn:hover {
  background: rgba(255, 255, 255, 0.2);
}

.modal-action-btn.close:hover {
  background: #ef4444;
}

.modal-content {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  padding: 20px 60px;
}

.modal-image-wrapper {
  max-width: 100%;
  max-height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-image-wrapper img {
  max-width: 100%;
  max-height: calc(100vh - 200px);
  object-fit: contain;
  border-radius: 8px;
}

.modal-nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.1);
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.modal-nav svg {
  width: 24px;
  height: 24px;
  color: #fff;
}

.modal-nav:hover {
  background: rgba(255, 255, 255, 0.2);
}

.modal-nav.prev {
  left: 15px;
}

.modal-nav.next {
  right: 15px;
}

.modal-thumbnails {
  display: flex;
  gap: 8px;
  padding: 15px 20px;
  background: rgba(0, 0, 0, 0.5);
  overflow-x: auto;
  justify-content: center;
}

.modal-thumb {
  width: 80px;
  height: 60px;
  border-radius: 6px;
  overflow: hidden;
  border: 2px solid transparent;
  cursor: pointer;
  transition: all 0.3s ease;
  flex-shrink: 0;
  opacity: 0.6;
}

.modal-thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.modal-thumb:hover {
  opacity: 1;
}

.modal-thumb.active {
  border-color: #10b981;
  opacity: 1;
}

/* Mobile Modal Adjustments */
@media (max-width: 767px) {
  .modal-content {
    padding: 10px 50px;
  }
  
  .modal-nav {
    width: 40px;
    height: 40px;
  }
  
  .modal-nav.prev {
    left: 5px;
  }
  
  .modal-nav.next {
    right: 5px;
  }
  
  .modal-thumbnails {
    padding: 10px 15px;
    justify-content: flex-start;
  }
  
  .modal-thumb {
    width: 60px;
    height: 45px;
  }
  
  .modal-image-wrapper img {
    max-height: calc(100vh - 180px);
  }
}
</style>

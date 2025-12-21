<template>
  <section class="showcase-section">
    <div class="container">
      <div class="section-header">
        <span class="section-tag">วิดีโอ & คอนเทนต์</span>
        <h2 class="section-title">รีวิว & เรื่องราวจากลูกค้า</h2>
        <p class="section-subtitle">ชมวิดีโอรีวิว บทสัมภาษณ์ และคอนเทนต์เกี่ยวกับอสังหาริมทรัพย์</p>
      </div>

      <!-- Category Tabs -->
      <div class="category-tabs">
        <button
          v-for="tab in tabs"
          :key="tab.id"
          :class="['tab-btn', { active: activeTab === tab.id }]"
          @click="activeTab = tab.id"
        >
          <span v-html="tab.icon"></span>
          {{ tab.name }}
        </button>
      </div>

      <!-- Video Grid -->
      <div class="video-grid">
        <div
          v-for="video in filteredVideos"
          :key="video.id"
          class="video-card"
          @click="openVideo(video)"
        >
          <div class="video-thumbnail">
            <img :src="video.thumbnail" :alt="video.title" />
            <div class="play-overlay">
              <div class="play-btn">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor">
                  <path d="M8 5v14l11-7z"/>
                </svg>
              </div>
              <span class="video-duration">{{ video.duration }}</span>
            </div>
            <span class="video-category">{{ getCategoryName(video.category) }}</span>
          </div>
          <div class="video-info">
            <h4 class="video-title">{{ video.title }}</h4>
            <div class="video-meta">
              <span class="video-views">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/>
                  <circle cx="12" cy="12" r="3"/>
                </svg>
                {{ formatViews(video.views) }}
              </span>
              <span class="video-date">{{ video.date }}</span>
            </div>
          </div>
        </div>
      </div>

      <!-- View More Button -->
      <div class="view-more">
        <NuxtLink to="/videos" class="view-more-btn">
          ดูวิดีโอทั้งหมด
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M5 12h14M12 5l7 7-7 7"/>
          </svg>
        </NuxtLink>
      </div>
    </div>

    <!-- Video Modal -->
    <Teleport to="body">
      <div :class="['video-modal-overlay', { active: isModalOpen }]" @click="closeModal">
        <div class="video-modal" @click.stop>
          <button class="modal-close" @click="closeModal">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <line x1="18" y1="6" x2="6" y2="18"/>
              <line x1="6" y1="6" x2="18" y2="18"/>
            </svg>
          </button>
          <div class="modal-video">
            <iframe
              v-if="currentVideo"
              :src="currentVideo.embedUrl"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
          </div>
          <div class="modal-info" v-if="currentVideo">
            <h3>{{ currentVideo.title }}</h3>
            <p>{{ currentVideo.description }}</p>
          </div>
        </div>
      </div>
    </Teleport>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue';

const activeTab = ref('all');
const isModalOpen = ref(false);
const currentVideo = ref(null);

const tabs = [
  { 
    id: 'all', 
    name: 'ทั้งหมด',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="7" height="7"/><rect x="14" y="3" width="7" height="7"/><rect x="14" y="14" width="7" height="7"/><rect x="3" y="14" width="7" height="7"/></svg>'
  },
  { 
    id: 'interview', 
    name: 'สัมภาษณ์ลูกค้า',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M22 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>'
  },
  { 
    id: 'review', 
    name: 'รีวิวคอนโด/บ้าน',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>'
  },
  { 
    id: 'content', 
    name: 'คอนเทนต์อสังหา',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14.5 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7.5L14.5 2z"/><polyline points="14 2 14 8 20 8"/></svg>'
  },
  { 
    id: 'renovation', 
    name: 'รีโนเวท',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"/></svg>'
  }
];

const videos = [
  {
    id: 1,
    title: 'คุณสมชาย - ซื้อคอนโดในฝันกับ DD Expert',
    category: 'interview',
    thumbnail: 'https://images.unsplash.com/photo-1560518883-ce09059eeffa?w=600',
    duration: '5:32',
    views: 12500,
    date: '2 วันก่อน',
    embedUrl: 'https://www.youtube.com/embed/dQw4w9WgXcQ?autoplay=1',
    description: 'บทสัมภาษณ์คุณสมชาย ลูกค้าที่ไว้วางใจให้ DD Expert ช่วยหาคอนโดในฝัน'
  },
  {
    id: 2,
    title: 'รีวิว Ideo Q สุขุมวิท 36 - คอนโดใกล้ BTS ทองหล่อ',
    category: 'review',
    thumbnail: 'https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=600',
    duration: '12:45',
    views: 45000,
    date: '1 สัปดาห์ก่อน',
    embedUrl: 'https://www.youtube.com/embed/dQw4w9WgXcQ?autoplay=1',
    description: 'พาชมห้องตัวอย่างและสิ่งอำนวยความสะดวกของโครงการ Ideo Q สุขุมวิท 36'
  },
  {
    id: 3,
    title: '5 เทคนิคเลือกซื้อคอนโดสำหรับมือใหม่',
    category: 'content',
    thumbnail: 'https://images.unsplash.com/photo-1560448204-e02f11c3d0e2?w=600',
    duration: '8:20',
    views: 89000,
    date: '2 สัปดาห์ก่อน',
    embedUrl: 'https://www.youtube.com/embed/dQw4w9WgXcQ?autoplay=1',
    description: 'เคล็ดลับสำคัญสำหรับคนที่กำลังจะซื้อคอนโดเป็นครั้งแรก'
  },
  {
    id: 4,
    title: 'รีโนเวทห้องงบ 50,000 บาท - Before & After',
    category: 'renovation',
    thumbnail: 'https://images.unsplash.com/photo-1600566753190-17f0baa2a6c3?w=600',
    duration: '15:30',
    views: 125000,
    date: '3 สัปดาห์ก่อน',
    embedUrl: 'https://www.youtube.com/embed/dQw4w9WgXcQ?autoplay=1',
    description: 'เปลี่ยนห้องเก่าให้กลายเป็นห้องใหม่สไตล์มินิมอลด้วยงบเพียง 50,000 บาท'
  },
  {
    id: 5,
    title: 'คุณวิภา - ขายบ้านได้ในราคาเกินความคาดหมาย',
    category: 'interview',
    thumbnail: 'https://images.unsplash.com/photo-1600596542815-ffad4c1539a9?w=600',
    duration: '6:15',
    views: 8500,
    date: '1 เดือนก่อน',
    embedUrl: 'https://www.youtube.com/embed/dQw4w9WgXcQ?autoplay=1',
    description: 'คุณวิภาเล่าประสบการณ์ขายบ้านกับทีมงาน DD Expert'
  },
  {
    id: 6,
    title: 'รีวิว The Line พหลโยธิน - High Rise 28 ชั้น',
    category: 'review',
    thumbnail: 'https://images.unsplash.com/photo-1600607687939-ce8a6c25118c?w=600',
    duration: '18:00',
    views: 67000,
    date: '1 เดือนก่อน',
    embedUrl: 'https://www.youtube.com/embed/dQw4w9WgXcQ?autoplay=1',
    description: 'พาชมโครงการ The Line พหลโยธิน คอนโด High Rise ใกล้ BTS หมอชิต'
  }
];

const filteredVideos = computed(() => {
  if (activeTab.value === 'all') {
    return videos;
  }
  return videos.filter(v => v.category === activeTab.value);
});

const getCategoryName = (category) => {
  const tab = tabs.find(t => t.id === category);
  return tab ? tab.name : '';
};

const formatViews = (views) => {
  if (views >= 1000) {
    return (views / 1000).toFixed(1) + 'K';
  }
  return views;
};

const openVideo = (video) => {
  currentVideo.value = video;
  isModalOpen.value = true;
  document.body.style.overflow = 'hidden';
};

const closeModal = () => {
  isModalOpen.value = false;
  currentVideo.value = null;
  document.body.style.overflow = '';
};
</script>

<style scoped>
.showcase-section {
  padding: 80px 0;
  background: linear-gradient(180deg, #1a1a1a 0%, #2a2a2a 100%);
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 20px;
}

.section-header {
  text-align: center;
  margin-bottom: 40px;
}

.section-tag {
  display: inline-block;
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
  color: #fff;
  padding: 8px 20px;
  border-radius: 30px;
  font-size: 0.85rem;
  font-weight: 600;
  margin-bottom: 15px;
}

.section-title {
  font-size: 2.5rem;
  font-weight: 700;
  color: #fff;
  margin: 0 0 15px 0;
}

.section-subtitle {
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.7);
  margin: 0;
}

/* Category Tabs */
.category-tabs {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 40px;
}

.tab-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: rgba(255, 255, 255, 0.7);
  padding: 10px 20px;
  border-radius: 30px;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.tab-btn :deep(svg) {
  width: 18px;
  height: 18px;
}

.tab-btn:hover {
  background: rgba(212, 175, 55, 0.2);
  border-color: rgba(212, 175, 55, 0.5);
  color: #fff;
}

.tab-btn.active {
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
  border-color: #d4af37;
  color: #fff;
}

/* Video Grid */
.video-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 25px;
}

.video-card {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 16px;
  overflow: hidden;
  cursor: pointer;
  transition: all 0.3s ease;
}

.video-card:hover {
  transform: translateY(-8px);
  background: rgba(255, 255, 255, 0.1);
}

.video-thumbnail {
  position: relative;
  aspect-ratio: 16/9;
  overflow: hidden;
}

.video-thumbnail img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.video-card:hover .video-thumbnail img {
  transform: scale(1.1);
}

.play-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.video-card:hover .play-overlay {
  opacity: 1;
}

.play-btn {
  width: 60px;
  height: 60px;
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 5px 20px rgba(212, 175, 55, 0.5);
}

.play-btn svg {
  width: 28px;
  height: 28px;
  color: #fff;
  margin-left: 4px;
}

.video-duration {
  position: absolute;
  bottom: 10px;
  right: 10px;
  background: rgba(0, 0, 0, 0.8);
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 0.75rem;
  font-weight: 500;
}

.video-category {
  position: absolute;
  top: 10px;
  left: 10px;
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
  color: #fff;
  padding: 4px 12px;
  border-radius: 4px;
  font-size: 0.7rem;
  font-weight: 600;
}

.video-info {
  padding: 20px;
}

.video-title {
  font-size: 1rem;
  font-weight: 600;
  color: #fff;
  margin: 0 0 10px 0;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.video-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: rgba(255, 255, 255, 0.5);
  font-size: 0.8rem;
}

.video-views {
  display: flex;
  align-items: center;
  gap: 5px;
}

.video-views svg {
  width: 14px;
  height: 14px;
}

/* View More */
.view-more {
  text-align: center;
  margin-top: 40px;
}

.view-more-btn {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  background: transparent;
  border: 2px solid #d4af37;
  color: #d4af37;
  padding: 14px 35px;
  border-radius: 50px;
  font-size: 1rem;
  font-weight: 600;
  text-decoration: none;
  transition: all 0.3s ease;
}

.view-more-btn svg {
  width: 20px;
  height: 20px;
  transition: transform 0.3s ease;
}

.view-more-btn:hover {
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
  color: #fff;
}

.view-more-btn:hover svg {
  transform: translateX(5px);
}

/* Video Modal */
.video-modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.95);
  z-index: 3000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 40px;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.video-modal-overlay.active {
  opacity: 1;
  visibility: visible;
}

.video-modal {
  width: 100%;
  max-width: 1000px;
  transform: scale(0.9);
  transition: transform 0.3s ease;
}

.video-modal-overlay.active .video-modal {
  transform: scale(1);
}

.modal-close {
  position: absolute;
  top: 20px;
  right: 20px;
  background: rgba(255, 255, 255, 0.1);
  border: none;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.modal-close svg {
  width: 24px;
  height: 24px;
  color: #fff;
}

.modal-close:hover {
  background: #d4af37;
}

.modal-video {
  position: relative;
  width: 100%;
  padding-top: 56.25%;
  border-radius: 16px;
  overflow: hidden;
  background: #000;
}

.modal-video iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.modal-info {
  padding: 25px 0;
  color: #fff;
}

.modal-info h3 {
  font-size: 1.3rem;
  font-weight: 600;
  margin: 0 0 10px 0;
}

.modal-info p {
  font-size: 0.95rem;
  color: rgba(255, 255, 255, 0.7);
  margin: 0;
}

/* Responsive */
@media (max-width: 991px) {
  .showcase-section {
    padding: 60px 0;
  }
  
  .section-title {
    font-size: 2rem;
  }
  
  .video-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 767px) {
  .category-tabs {
    overflow-x: auto;
    flex-wrap: nowrap;
    justify-content: flex-start;
    padding-bottom: 10px;
    -webkit-overflow-scrolling: touch;
  }
  
  .tab-btn {
    flex-shrink: 0;
    padding: 8px 16px;
    font-size: 0.85rem;
  }
}

@media (max-width: 575px) {
  .section-title {
    font-size: 1.6rem;
  }
  
  .video-grid {
    grid-template-columns: 1fr;
    gap: 20px;
  }
  
  .video-modal-overlay {
    padding: 20px;
  }
}
</style>

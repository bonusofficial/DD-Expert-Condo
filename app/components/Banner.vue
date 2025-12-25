<template>
  <section class="banner-section">
    <Swiper
      :modules="[SwiperAutoplay, SwiperPagination, SwiperEffectFade]"
      :slides-per-view="1"
      :loop="true"
      :autoplay="{
        delay: 5000,
        disableOnInteraction: false,
        pauseOnMouseEnter: true,
      }"
      :pagination="{
        clickable: true,
        dynamicBullets: true,
      }"
      :effect="'fade'"
      :fadeEffect="{ crossFade: true }"
      class="banner-swiper"
      @slideChange="onSlideChange"
    >
      <SwiperSlide v-for="(slide, index) in slides" :key="index">
        <!-- Image Slide -->
        <div v-if="slide.type === 'image'" class="slide-content">
          <img :src="slide.src" :alt="slide.alt || ''" class="slide-image" />
        </div>

        <!-- Video Slide (YouTube) -->
        <div
          v-else-if="slide.type === 'youtube'"
          class="slide-content video-slide"
          @click="playVideo(index)"
        >
          <div class="video-thumbnail">
            <img
              :src="getYoutubeThumbnail(slide.src)"
              :alt="slide.alt || ''"
              class="slide-image"
            />
            <div class="play-overlay">
              <div class="play-icon">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="currentColor"
                >
                  <path d="M8 5v14l11-7z" />
                </svg>
              </div>
            </div>
          </div>
        </div>

        <!-- Video Slide (MP4/General) -->
        <div
          v-else-if="slide.type === 'video'"
          class="slide-content video-slide"
        >
          <video
            :ref="(el) => (videoRefs[index] = el)"
            :src="slide.src"
            :poster="slide.poster"
            class="slide-video"
            muted
            loop
            playsinline
            autoplay
          ></video>
        </div>
      </SwiperSlide>
    </Swiper>

    <!-- Search Section Overlay -->
    <div class="search-section">
      <div class="search-container">
        <!-- Hero Text -->
        <div class="hero-text">
          <h1 class="hero-title">
            เว็บประกาศ ขาย ให้เช่า คอนโด บ้าน/ที่ดิน ยอดนิยม ดีที่สุด
            <span class="highlight">ลงประกาศฟรี</span>
          </h1>
          <p class="hero-subtitle">
            คอนโดหรู เช่าคอนโด ขายคอนโด เช่าบ้าน ขายบ้าน ขายที่ดิน
          </p>
        </div>

        <!-- Filter Tabs -->
        <div class="filter-tabs">
          <button
            v-for="tab in filterTabs"
            :key="tab.value"
            :class="['filter-tab', { active: activeFilter === tab.value }]"
            @click="activeFilter = tab.value"
          >
            <span v-html="tab.icon"></span>
            {{ tab.label }}
          </button>
        </div>

        <!-- Search Input -->
        <div class="search-input-wrapper">
          <div class="search-input-box">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              class="search-icon"
            >
              <circle cx="11" cy="11" r="8" />
              <path d="m21 21-4.35-4.35" />
            </svg>
            <input
              type="text"
              v-model="searchQuery"
              placeholder="กรอกชื่อ ทำเล /โครงการ /รถไฟฟ้า"
              class="search-input"
            />
            <button class="search-btn" @click="performSearch">
              ค้นหา
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <path d="M5 12h14M12 5l7 7-7 7" />
              </svg>
            </button>
          </div>
        </div>

        <!-- Property Categories -->
        <div class="property-categories">
          <h3 class="categories-title">ประเภทอสังหาริมทรัพย์</h3>
          <div class="categories-grid">
            <NuxtLink
              v-for="category in propertyCategories"
              :key="category.name"
              :to="category.link"
              class="category-item"
            >
              <div class="category-icon-wrapper">
                <span v-html="category.icon"></span>
              </div>
              <span class="category-name">{{ category.name }}</span>
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>

    <!-- Video Modal (for YouTube) -->
    <Teleport to="body">
      <div
        :class="['video-modal-overlay', { active: isVideoModalOpen }]"
        @click="closeVideoModal"
      >
        <div class="video-modal" @click.stop>
          <button class="video-modal-close" @click="closeVideoModal">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
            >
              <line x1="18" y1="6" x2="6" y2="18" />
              <line x1="6" y1="6" x2="18" y2="18" />
            </svg>
          </button>
          <div class="video-modal-content">
            <iframe
              v-if="currentVideoUrl"
              :src="currentVideoUrl"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
          </div>
        </div>
      </div>
    </Teleport>
  </section>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { Swiper, SwiperSlide } from "swiper/vue";
import {
  Autoplay as SwiperAutoplay,
  Pagination as SwiperPagination,
  EffectFade as SwiperEffectFade,
} from "swiper/modules";

// Import Swiper styles
import "swiper/css";
import "swiper/css/pagination";
import "swiper/css/navigation";
import "swiper/css/effect-fade";

// Props
const props = defineProps({
  slides: {
    type: Array,
    default: () => [
      {
        type: "image",
        src: "https://images.unsplash.com/photo-1545324418-cc1a3fa10c00?w=1920",
        title: "คอนโดหรูใจกลางเมือง",
        description:
          "พบกับคอนโดมิเนียมระดับพรีเมียม ทำเลดี ใกล้ BTS เดินทางสะดวก",
        tag: "แนะนำ",
        link: "/condo",
        buttonText: "ดูโครงการ",
      },
      {
        type: "image",
        src: "https://images.unsplash.com/photo-1600596542815-ffad4c1539a9?w=1920",
        title: "บ้านเดี่ยวสุดหรู",
        description:
          "บ้านเดี่ยวดีไซน์โมเดิร์น พื้นที่กว้างขวาง เหมาะสำหรับครอบครัว",
        tag: "ยอดนิยม",
        link: "/house",
        buttonText: "ดูรายละเอียด",
      },
      {
        type: "youtube",
        src: "https://www.youtube.com/watch?v=dQw4w9WgXcQ",
        title: "ทัวร์โครงการใหม่",
        description: "ชมวิดีโอแนะนำโครงการใหม่ล่าสุดจาก DD Expert Condo",
        tag: "วิดีโอ",
      },
      {
        type: "image",
        src: "https://images.unsplash.com/photo-1600607687939-ce8a6c25118c?w=1920",
        title: "ที่ดินทำเลทอง",
        description: "ที่ดินพร้อมขาย ทำเลศักยภาพสูง เหมาะสำหรับการลงทุน",
        tag: "โปรโมชั่น",
        link: "/land",
        buttonText: "ติดต่อสอบถาม",
      },
    ],
  },
});

// Video refs
const videoRefs = ref({});
const currentSlideIndex = ref(0);
const isVideoModalOpen = ref(false);
const currentVideoUrl = ref("");

// Search Section Data
const searchQuery = ref("");
const activeFilter = ref("buy");

const filterTabs = [
  {
    value: "buy",
    label: "ขาย",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/></svg>',
  },
  {
    value: "rent",
    label: "หาเช่า",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect width="18" height="18" x="3" y="4" rx="2" ry="2"/><line x1="16" x2="16" y1="2" y2="6"/><line x1="8" x2="8" y1="2" y2="6"/></svg>',
  },
  {
    value: "all",
    label: "ปรศเตรวม",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect width="7" height="7" x="3" y="3" rx="1"/><rect width="7" height="7" x="14" y="3" rx="1"/><rect width="7" height="7" x="14" y="14" rx="1"/><rect width="7" height="7" x="3" y="14" rx="1"/></svg>',
  },
  {
    value: "agent",
    label: "ทรัพย์ใกล้ฉัน",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>',
  },
  {
    value: "bts",
    label: "คอนโดใกล้ BTS",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M8 6v6M16 6v6M4 12h16M5.5 18h13a1 1 0 0 0 1-1v-5a1 1 0 0 0-1-1h-13a1 1 0 0 0-1 1v5a1 1 0 0 0 1 1zM9 18v2M15 18v2"/></svg>',
  },
  {
    value: "mrt",
    label: "คอนโดใกล้ MRT",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M8 6v6M16 6v6M4 12h16M5.5 18h13a1 1 0 0 0 1-1v-5a1 1 0 0 0-1-1h-13a1 1 0 0 0-1 1v5a1 1 0 0 0 1 1zM9 18v2M15 18v2"/></svg>',
  },
];

const propertyCategories = [
  {
    name: "คอนโด",
    link: "/condo",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="4" y="2" width="16" height="20" rx="2" ry="2"/><path d="M9 22v-4h6v4"/><path d="M8 6h.01M16 6h.01M12 6h.01M8 10h.01M16 10h.01M12 10h.01M8 14h.01M16 14h.01M12 14h.01"/></svg>',
  },
  {
    name: "บ้าน",
    link: "/house",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>',
  },
  {
    name: "ทาวน์โฮม",
    link: "/townhouse",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 21h18"/><path d="M5 21V7l8-4v18"/><path d="M19 21V11l-6-4"/></svg>',
  },
  {
    name: "บ้านแฝด",
    link: "/twin-house",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 21h18"/><path d="M4 21V10l8-6 8 6v11"/><line x1="12" y1="4" x2="12" y2="21"/></svg>',
  },
  {
    name: "ที่ดิน",
    link: "/land",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"/></svg>',
  },
  {
    name: "ตึกแถว",
    link: "/shophouse",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M6 22V4a2 2 0 0 1 2-2h8a2 2 0 0 1 2 2v18Z"/><path d="M6 12H4a2 2 0 0 0-2 2v6a2 2 0 0 0 2 2h2"/><path d="M18 9h2a2 2 0 0 1 2 2v9a2 2 0 0 1-2 2h-2"/></svg>',
  },
  {
    name: "สำนักงาน",
    link: "/office",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="2" y="7" width="20" height="14" rx="2" ry="2"/><path d="M16 21V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v16"/></svg>',
  },
  {
    name: "โฮมออฟฟิศ",
    link: "/home-office",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><rect x="9" y="12" width="6" height="7"/></svg>',
  },
  {
    name: "ร้านค้า",
    link: "/retail",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="m2 7 4.41-4.41A2 2 0 0 1 7.83 2h8.34a2 2 0 0 1 1.42.59L22 7"/><path d="M4 12v8a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2v-8"/><path d="M15 22v-4a2 2 0 0 0-2-2h-2a2 2 0 0 0-2 2v4"/><path d="M2 7h20"/></svg>',
  },
  {
    name: "โกดัง",
    link: "/warehouse",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 21h18"/><path d="M3 7v1a3 3 0 0 0 6 0V7m0 0V4l3-1 3 1v3m0 0v1a3 3 0 0 0 6 0V7"/><path d="M21 21V7"/><path d="M3 21V7"/></svg>',
  },
  {
    name: "โรงแรม",
    link: "/hotel",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V4a2 2 0 0 0-2-2Z"/><path d="m9 16 .348-.24c1.465-1.013 3.84-1.013 5.304 0L15 16"/><path d="M8 7h.01M16 7h.01M12 7h.01M8 11h.01M16 11h.01M12 11h.01"/></svg>',
  },
  {
    name: "Co-Working",
    link: "/coworking",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M22 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>',
  },
];

const performSearch = () => {
  console.log("Search:", {
    query: searchQuery.value,
    filter: activeFilter.value,
  });
  // Navigate to search results page
  // navigateTo(`/search?q=${searchQuery.value}&type=${activeFilter.value}`);
};

// Get YouTube embed URL
const getYoutubeEmbedUrl = (url) => {
  if (!url) return "";
  const videoId = url.match(
    /(?:youtube\.com\/(?:[^\/]+\/.+\/|(?:v|e(?:mbed)?)\/|.*[?&]v=)|youtu\.be\/)([^"&?\/\s]{11})/
  );
  if (videoId && videoId[1]) {
    return `https://www.youtube.com/embed/${videoId[1]}?rel=0&modestbranding=1`;
  }
  return url;
};

// Get YouTube thumbnail URL
const getYoutubeThumbnail = (url) => {
  if (!url) return "";
  const videoId = url.match(
    /(?:youtube\.com\/(?:[^\/]+\/.+\/|(?:v|e(?:mbed)?)\/|.*[?&]v=)|youtu\.be\/)([^"&?\/\s]{11})/
  );
  if (videoId && videoId[1]) {
    return `https://img.youtube.com/vi/${videoId[1]}/maxresdefault.jpg`;
  }
  return "";
};

// Get YouTube embed URL with autoplay
const getYoutubeAutoplayUrl = (url) => {
  const baseUrl = getYoutubeEmbedUrl(url);
  return baseUrl ? `${baseUrl}&autoplay=1` : "";
};

// Play video in modal
const playVideo = (index) => {
  const slide = props.slides[index];
  if (slide.type === "youtube") {
    currentVideoUrl.value = getYoutubeAutoplayUrl(slide.src);
    isVideoModalOpen.value = true;
    document.body.style.overflow = "hidden";
  }
};

// Close video modal
const closeVideoModal = () => {
  isVideoModalOpen.value = false;
  currentVideoUrl.value = "";
  document.body.style.overflow = "";
};

// Handle slide change
const onSlideChange = (swiper) => {
  currentSlideIndex.value = swiper.realIndex;

  // Pause all videos
  Object.values(videoRefs.value).forEach((video) => {
    if (video && video.pause) {
      video.pause();
    }
  });

  // Play current slide video if it's a video type
  const currentSlide = props.slides[swiper.realIndex];
  if (currentSlide?.type === "video") {
    const video = videoRefs.value[swiper.realIndex];
    if (video && video.play) {
      video.play().catch(() => {});
    }
  }
};

// Close modal on escape
onMounted(() => {
  if (typeof window !== "undefined") {
    window.addEventListener("keydown", (e) => {
      if (e.key === "Escape" && isVideoModalOpen.value) {
        closeVideoModal();
      }
    });
  }
});
</script>

<style scoped>
.banner-section {
  position: relative;
  width: 100%;
  background: #1a1a1a;
}

/* Search Section */
.search-section {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 100;
  padding: 40px 20px 30px;
  background: linear-gradient(
    to top,
    rgba(0, 0, 0, 0.95) 0%,
    rgba(0, 0, 0, 0.8) 50%,
    rgba(0, 0, 0, 0.4) 100%
  );
}

.search-container {
  max-width: 1200px;
  margin: 0 auto;
}

/* Hero Text */
.hero-text {
  text-align: center;
  margin-bottom: 25px;
}

.hero-title {
  font-size: 1.8rem;
  font-weight: 700;
  color: #fff;
  margin: 0 0 10px 0;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.5);
}

.hero-title .highlight {
  color: #DAA520;
}

.hero-subtitle {
  font-size: 0.95rem;
  color: rgba(255, 255, 255, 0.7);
  margin: 0;
}

/* Filter Tabs */
.filter-tabs {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 20px;
}

.filter-tab {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 8px 16px;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 25px;
  color: rgba(255, 255, 255, 0.8);
  font-size: 0.85rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
}

.filter-tab:hover {
  background: rgba(218, 165, 32, 0.2);
  border-color: rgba(218, 165, 32, 0.5);
  color: #DAA520;
}

.filter-tab.active {
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  border-color: #DAA520;
  color: #fff;
}

.filter-tab :deep(svg) {
  width: 16px;
  height: 16px;
}

/* Search Input Wrapper */
.search-input-wrapper {
  max-width: 800px;
  margin: 0 auto 30px;
}

.search-input-box {
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 50px;
  padding: 8px 8px 8px 20px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
}

.search-icon {
  width: 24px;
  height: 24px;
  color: #999;
  flex-shrink: 0;
}

.search-input {
  flex: 1;
  border: none;
  outline: none;
  padding: 12px 15px;
  font-size: 1rem;
  color: #333;
  background: transparent;
}

.search-input::placeholder {
  color: #999;
}

.search-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  color: #fff;
  border: none;
  padding: 14px 28px;
  border-radius: 50px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.search-btn svg {
  width: 18px;
  height: 18px;
}

.search-btn:hover {
  background: linear-gradient(135deg, #b8860b 0%, #8b7d3a 100%);
  transform: translateX(3px);
}

/* Property Categories */
.property-categories {
  text-align: center;
}

.categories-title {
  font-size: 1rem;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.9);
  margin: 0 0 20px 0;
}

.categories-grid {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 15px;
}

.category-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  padding: 12px 8px;
  min-width: 70px;
  text-decoration: none;
  transition: all 0.3s ease;
}

.category-item:hover {
  transform: translateY(-5px);
}

.category-icon-wrapper {
  width: 50px;
  height: 50px;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.15);
}

.category-item:hover .category-icon-wrapper {
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  border-color: #DAA520;
}

.category-icon-wrapper :deep(svg) {
  width: 24px;
  height: 24px;
  color: rgba(255, 255, 255, 0.8);
  transition: all 0.3s ease;
}

.category-item:hover .category-icon-wrapper :deep(svg) {
  color: #fff;
}

.category-name {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.8);
  text-align: center;
  white-space: nowrap;
}

.category-item:hover .category-name {
  color: #DAA520;
}

.banner-swiper {
  width: 100%;
  height: 70vh;
  min-height: 500px;
  max-height: 800px;
}

.slide-content {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.slide-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

.slide-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

.video-thumbnail {
  position: relative;
  width: 100%;
  height: 100%;
  cursor: pointer;
}

.play-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.video-thumbnail:hover .play-overlay {
  background: rgba(0, 0, 0, 0.5);
}

.play-icon {
  width: 80px;
  height: 80px;
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 8px 30px rgba(212, 175, 55, 0.5);
  transition: all 0.3s ease;
}

.video-thumbnail:hover .play-icon {
  transform: scale(1.1);
  box-shadow: 0 12px 40px rgba(212, 175, 55, 0.6);
}

.play-icon svg {
  width: 35px;
  height: 35px;
  color: #fff;
  margin-left: 5px;
}

.video-wrapper {
  position: relative;
  width: 100%;
  height: 100%;
  background: #000;
}

.video-wrapper iframe {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100%;
  height: 100%;
  pointer-events: none;
}

.slide-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(
    to right,
    rgba(0, 0, 0, 0.7) 0%,
    rgba(0, 0, 0, 0.4) 50%,
    rgba(0, 0, 0, 0.2) 100%
  );
}

.video-overlay {
  background: linear-gradient(
    to right,
    rgba(0, 0, 0, 0.8) 0%,
    rgba(0, 0, 0, 0.5) 50%,
    rgba(0, 0, 0, 0.3) 100%
  );
}

.slide-caption {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 60px 80px;
  z-index: 10;
  max-width: 700px;
}

.slide-tag {
  display: inline-block;
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  color: #fff;
  padding: 6px 16px;
  border-radius: 20px;
  font-size: 0.85rem;
  font-weight: 600;
  margin-bottom: 15px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.slide-title {
  font-size: 3rem;
  font-weight: 700;
  color: #fff;
  margin: 0 0 15px 0;
  line-height: 1.2;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}

.slide-description {
  font-size: 1.2rem;
  color: rgba(255, 255, 255, 0.9);
  margin: 0 0 25px 0;
  line-height: 1.6;
  max-width: 500px;
}

.slide-btn {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  color: #fff;
  padding: 14px 30px;
  border-radius: 50px;
  font-size: 1rem;
  font-weight: 600;
  text-decoration: none;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 20px rgba(212, 175, 55, 0.4);
}

.slide-btn svg {
  width: 20px;
  height: 20px;
  transition: transform 0.3s ease;
}

.slide-btn:hover {
  background: linear-gradient(135deg, #f4e5b2 0%, #DAA520 100%);
  color: #1a1a1a;
  transform: translateY(-2px);
  box-shadow: 0 6px 25px rgba(212, 175, 55, 0.5);
}

.slide-btn:hover svg {
  transform: translateX(5px);
}

.play-btn svg {
  width: 24px;
  height: 24px;
}

/* Swiper Custom Styles */
:deep(.swiper-pagination) {
  bottom: 25px !important;
}

:deep(.swiper-pagination-bullet) {
  width: 12px;
  height: 12px;
  background: rgba(255, 255, 255, 0.5);
  opacity: 1;
  transition: all 0.3s ease;
}

:deep(.swiper-pagination-bullet-active) {
  background: #DAA520;
  width: 35px;
  border-radius: 6px;
}

:deep(.swiper-button-prev),
:deep(.swiper-button-next) {
  width: 50px;
  height: 50px;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  border-radius: 50%;
  transition: all 0.3s ease;
}

:deep(.swiper-button-prev):hover,
:deep(.swiper-button-next):hover {
  background: #DAA520;
}

:deep(.swiper-button-prev)::after,
:deep(.swiper-button-next)::after {
  font-size: 18px;
  font-weight: 700;
  color: #fff;
}

:deep(.swiper-button-prev) {
  left: 25px;
}

:deep(.swiper-button-next) {
  right: 25px;
}

/* Video Modal */
.video-modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.9);
  z-index: 3000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.video-modal-overlay.active {
  opacity: 1;
  visibility: visible;
}

.video-modal {
  position: relative;
  width: 100%;
  max-width: 1200px;
  background: #000;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 25px 80px rgba(0, 0, 0, 0.5);
  transform: scale(0.9);
  transition: all 0.3s ease;
}

.video-modal-overlay.active .video-modal {
  transform: scale(1);
}

.video-modal-close {
  position: absolute;
  top: -50px;
  right: 0;
  background: transparent;
  border: 2px solid rgba(255, 255, 255, 0.3);
  width: 44px;
  height: 44px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  z-index: 10;
}

.video-modal-close svg {
  width: 20px;
  height: 20px;
  color: #fff;
}

.video-modal-close:hover {
  background: #DAA520;
  border-color: #DAA520;
}

.video-modal-content {
  position: relative;
  width: 100%;
  padding-top: 56.25%; /* 16:9 */
}

.video-modal-content iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

/* Responsive */
@media (max-width: 1199px) {
  .banner-swiper {
    height: 60vh;
    min-height: 450px;
  }

  .slide-caption {
    padding: 50px 60px;
  }

  .slide-title {
    font-size: 2.5rem;
  }

  .slide-description {
    font-size: 1.1rem;
  }
}

@media (max-width: 991px) {
  .banner-swiper {
    height: 55vh;
    min-height: 400px;
  }

  /* Search Section Responsive */
  .search-section {
    padding: 30px 15px 25px;
  }

  .hero-title {
    font-size: 1.5rem;
  }

  .hero-subtitle {
    font-size: 0.85rem;
  }

  .filter-tabs {
    gap: 6px;
  }

  .filter-tab {
    padding: 6px 12px;
    font-size: 0.75rem;
  }

  .filter-tab :deep(svg) {
    width: 14px;
    height: 14px;
  }

  .search-input-box {
    padding: 6px 6px 6px 15px;
  }

  .search-input {
    padding: 10px 12px;
    font-size: 0.9rem;
  }

  .search-btn {
    padding: 12px 20px;
    font-size: 0.9rem;
  }

  .categories-grid {
    gap: 10px;
  }

  .category-item {
    min-width: 60px;
    padding: 8px 5px;
  }

  .category-icon-wrapper {
    width: 45px;
    height: 45px;
  }

  .category-icon-wrapper :deep(svg) {
    width: 20px;
    height: 20px;
  }

  .category-name {
    font-size: 0.7rem;
  }

  .slide-caption {
    padding: 40px;
    max-width: 100%;
  }

  .slide-title {
    font-size: 2rem;
  }

  .slide-description {
    font-size: 1rem;
  }

  :deep(.swiper-button-prev),
  :deep(.swiper-button-next) {
    width: 40px;
    height: 40px;
  }

  :deep(.swiper-button-prev)::after,
  :deep(.swiper-button-next)::after {
    font-size: 14px;
  }
}

@media (max-width: 767px) {
  .banner-swiper {
    height: 50vh;
    min-height: 350px;
    max-height: 500px;
  }

  /* Search Section Mobile */
  .search-section {
    position: relative;
    padding: 25px 15px 20px;
    background: linear-gradient(180deg, #1a1a1a 0%, #2c2c2c 100%);
  }

  .hero-title {
    font-size: 1.2rem;
  }

  .hero-subtitle {
    font-size: 0.8rem;
  }

  .filter-tabs {
    overflow-x: auto;
    flex-wrap: nowrap;
    justify-content: flex-start;
    padding-bottom: 5px;
    -webkit-overflow-scrolling: touch;
  }

  .filter-tab {
    flex-shrink: 0;
    padding: 6px 10px;
    font-size: 0.7rem;
  }

  .search-input-wrapper {
    margin-bottom: 20px;
  }

  .search-input-box {
    flex-direction: column;
    border-radius: 12px;
    padding: 0;
    gap: 0;
  }

  .search-icon {
    display: none;
  }

  .search-input {
    width: 100%;
    padding: 14px 15px;
    border-radius: 12px 12px 0 0;
    border-bottom: 1px solid #eee;
  }

  .search-btn {
    width: 100%;
    justify-content: center;
    border-radius: 0 0 12px 12px;
    padding: 14px;
  }

  .categories-title {
    font-size: 0.9rem;
    margin-bottom: 15px;
  }

  .categories-grid {
    gap: 8px;
    justify-content: flex-start;
    overflow-x: auto;
    flex-wrap: nowrap;
    padding-bottom: 10px;
    -webkit-overflow-scrolling: touch;
  }

  .category-item {
    flex-shrink: 0;
    min-width: 55px;
  }

  .category-icon-wrapper {
    width: 40px;
    height: 40px;
    border-radius: 10px;
  }

  .category-icon-wrapper :deep(svg) {
    width: 18px;
    height: 18px;
  }

  .category-name {
    font-size: 0.65rem;
  }

  .slide-overlay {
    background: linear-gradient(
      to top,
      rgba(0, 0, 0, 0.8) 0%,
      rgba(0, 0, 0, 0.4) 50%,
      rgba(0, 0, 0, 0.2) 100%
    );
  }

  .slide-caption {
    padding: 25px 20px;
    text-align: center;
  }

  .slide-tag {
    font-size: 0.75rem;
    padding: 5px 12px;
  }

  .slide-title {
    font-size: 1.6rem;
    margin-bottom: 10px;
  }

  .slide-description {
    font-size: 0.9rem;
    margin-bottom: 20px;
    max-width: 100%;
  }

  .slide-btn {
    padding: 12px 24px;
    font-size: 0.9rem;
  }

  :deep(.swiper-button-prev),
  :deep(.swiper-button-next) {
    display: none;
  }

  :deep(.swiper-pagination) {
    bottom: 15px !important;
  }

  :deep(.swiper-pagination-bullet) {
    width: 10px;
    height: 10px;
  }

  :deep(.swiper-pagination-bullet-active) {
    width: 28px;
  }
}

@media (max-width: 575px) {
  .banner-swiper {
    height: 45vh;
    min-height: 300px;
  }

  .search-section {
    padding: 20px 10px 15px;
  }

  .hero-text {
    margin-bottom: 15px;
  }

  .hero-title {
    font-size: 1rem;
  }

  .filter-tabs {
    margin-bottom: 15px;
  }

  .search-input-wrapper {
    margin-bottom: 15px;
  }

  .slide-caption {
    padding: 20px 15px;
  }

  .slide-title {
    font-size: 1.4rem;
  }

  .slide-description {
    font-size: 0.85rem;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  .slide-btn {
    padding: 10px 20px;
    font-size: 0.85rem;
  }

  .slide-btn svg {
    width: 16px;
    height: 16px;
  }

  .video-modal-close {
    top: -45px;
    width: 38px;
    height: 38px;
  }
}
</style>

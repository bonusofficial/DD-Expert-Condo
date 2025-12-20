<template>
  <section class="banner-section">
    <Swiper
      :modules="[
        SwiperAutoplay,
        SwiperPagination,
        SwiperNavigation,
        SwiperEffectFade,
      ]"
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
      :navigation="true"
      :effect="'fade'"
      :fadeEffect="{ crossFade: true }"
      class="banner-swiper"
      @slideChange="onSlideChange"
    >
      <SwiperSlide v-for="(slide, index) in slides" :key="index">
        <!-- Image Slide -->
        <div v-if="slide.type === 'image'" class="slide-content">
          <img :src="slide.src" :alt="slide.title" class="slide-image" />
          <div class="slide-overlay"></div>
          <div class="slide-caption">
            <span class="slide-tag" v-if="slide.tag">{{ slide.tag }}</span>
            <h2 class="slide-title">{{ slide.title }}</h2>
            <p class="slide-description">{{ slide.description }}</p>
            <NuxtLink v-if="slide.link" :to="slide.link" class="slide-btn">
              {{ slide.buttonText || "ดูรายละเอียด" }}
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <path d="M5 12h14M12 5l7 7-7 7" />
              </svg>
            </NuxtLink>
          </div>
        </div>

        <!-- Video Slide (YouTube) -->
        <div
          v-else-if="slide.type === 'youtube'"
          class="slide-content video-slide"
        >
          <div class="video-wrapper">
            <iframe
              :src="getYoutubeEmbedUrl(slide.src)"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
          </div>
          <div class="slide-overlay video-overlay"></div>
          <div class="slide-caption">
            <span class="slide-tag" v-if="slide.tag">{{ slide.tag }}</span>
            <h2 class="slide-title">{{ slide.title }}</h2>
            <p class="slide-description">{{ slide.description }}</p>
            <button
              v-if="slide.src"
              class="slide-btn play-btn"
              @click="playVideo(index)"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="currentColor"
              >
                <path d="M8 5v14l11-7z" />
              </svg>
              ดูวิดีโอ
            </button>
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
          ></video>
          <div class="slide-overlay"></div>
          <div class="slide-caption">
            <span class="slide-tag" v-if="slide.tag">{{ slide.tag }}</span>
            <h2 class="slide-title">{{ slide.title }}</h2>
            <p class="slide-description">{{ slide.description }}</p>
            <NuxtLink v-if="slide.link" :to="slide.link" class="slide-btn">
              {{ slide.buttonText || "ดูรายละเอียด" }}
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <path d="M5 12h14M12 5l7 7-7 7" />
              </svg>
            </NuxtLink>
          </div>
        </div>
      </SwiperSlide>
    </Swiper>

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
  Navigation as SwiperNavigation,
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
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
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
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
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
  background: linear-gradient(135deg, #f4e5b2 0%, #d4af37 100%);
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
  background: #d4af37;
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
  background: #d4af37;
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
  background: #d4af37;
  border-color: #d4af37;
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

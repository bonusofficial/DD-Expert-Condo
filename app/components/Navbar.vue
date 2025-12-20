<template>
  <header class="header">
    <!-- Top Bar - Language & Quick Links -->
    <div class="top-bar">
      <div class="top-bar-container">
        <div class="top-bar-left">
          <span class="welcome-text">ยินดีต้อนรับสู่ DD Expert Condo</span>
        </div>
        <div class="top-bar-right">
          <!-- Language & Currency Dropdown -->
          <div class="lang-dropdown" :class="{ open: isLangDropdownOpen }">
            <button class="lang-dropdown-trigger" @click="toggleLangDropdown">
              <span class="lang-flag">{{ currentLanguage.flag }}</span>
              <span class="lang-name">{{ currentLanguage.name }}</span>
              <span class="lang-currency">{{ currentLanguage.currency }}</span>
              <svg
                class="dropdown-arrow"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <polyline points="6 9 12 15 18 9" />
              </svg>
            </button>
            <div class="lang-dropdown-menu">
              <button
                v-for="lang in languages"
                :key="lang.code"
                :class="[
                  'lang-dropdown-item',
                  { active: currentLang === lang.code },
                ]"
                @click="selectLang(lang.code)"
              >
                <span class="lang-flag">{{ lang.flag }}</span>
                <div class="lang-info">
                  <span class="lang-name">{{ lang.name }}</span>
                  <span class="lang-currency-name">{{
                    lang.currencyName
                  }}</span>
                </div>
                <span class="lang-currency-code">{{ lang.currency }}</span>
                <svg
                  v-if="currentLang === lang.code"
                  class="lang-check"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="3"
                >
                  <polyline points="20 6 9 17 4 12" />
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Navigation Bar with Logo -->
    <nav class="nav-bar">
      <div class="nav-container">
        <!-- Logo in Nav -->
        <div class="nav-logo">
          <NuxtLink to="/" class="logo">
            <img src="/logo.png" class="logo-img" alt="DDexpert" />
          </NuxtLink>
        </div>

        <!-- Navigation Menu -->
        <ul class="nav-menu">
          <li v-for="item in menuItems" :key="item.name" class="nav-item">
            <NuxtLink :to="item.link" class="nav-link">
              <span class="nav-icon" v-html="item.icon"></span>
              {{ item.name }}
            </NuxtLink>
          </li>
        </ul>

        <!-- Search Trigger Button -->
        <button class="search-trigger" @click="openSearchModal">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <circle cx="11" cy="11" r="8" />
            <path d="m21 21-4.35-4.35" />
          </svg>
          <span class="search-trigger-text">ค้นหา</span>
        </button>

        <!-- Contact Info -->
        <div class="nav-contact">
          <a href="tel:02-XXX-XXXX" class="contact-link">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
            >
              <path
                d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"
              />
            </svg>
            <span>02-XXX-XXXX</span>
          </a>
        </div>

        <!-- Mobile Menu Toggle -->
        <button
          :class="['mobile-toggle', { active: isMobileMenuOpen }]"
          @click="toggleMobileMenu"
        >
          <span class="hamburger"></span>
        </button>
      </div>
    </nav>

    <!-- Mobile Menu -->
    <div :class="['mobile-menu', { open: isMobileMenuOpen }]">
      <!-- Mobile Search Trigger -->
      <button class="mobile-search-trigger" @click="openSearchModal">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
        >
          <circle cx="11" cy="11" r="8" />
          <path d="m21 21-4.35-4.35" />
        </svg>
        <span>ค้นหาอสังหาริมทรัพย์</span>
      </button>

      <ul class="mobile-nav-list">
        <li v-for="item in menuItems" :key="item.name" class="mobile-nav-item">
          <NuxtLink
            :to="item.link"
            class="mobile-nav-link"
            @click="isMobileMenuOpen = false"
          >
            <span class="mobile-nav-icon" v-html="item.icon"></span>
            {{ item.name }}
          </NuxtLink>
        </li>
      </ul>

      <!-- Mobile Contact -->
      <div class="mobile-contact">
        <a href="tel:02-XXX-XXXX" class="mobile-contact-link">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72"
            />
          </svg>
          โทร 02-XXX-XXXX
        </a>
      </div>
    </div>

    <!-- Search Modal -->
    <Teleport to="body">
      <div
        :class="['search-modal-overlay', { active: isSearchModalOpen }]"
        @click="closeSearchModal"
      >
        <div class="search-modal" @click.stop>
          <!-- Modal Header -->
          <div class="modal-header">
            <div class="modal-logo">
              <img src="/logo.png" class="modal-logo-img" alt="DDexpert" />
            </div>
            <button class="modal-close" @click="closeSearchModal">
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
          </div>

          <!-- Modal Body -->
          <div class="modal-body">
            <h2 class="modal-title">ค้นหาอสังหาริมทรัพย์</h2>
            <p class="modal-subtitle">ค้นหาบ้าน คอนโด ที่ดิน ที่ใช่สำหรับคุณ</p>

            <!-- Search Input -->
            <div class="modal-search-input">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <circle cx="11" cy="11" r="8" />
                <path d="m21 21-4.35-4.35" />
              </svg>
              <input
                type="text"
                v-model="searchQuery"
                placeholder="ค้นหาชื่อโครงการ, ทำเลที่ตั้ง..."
                ref="searchInputRef"
              />
            </div>

            <!-- Search Filters -->
            <div class="modal-filters">
              <div class="filter-group">
                <label class="filter-label">ทำเลที่ตั้ง</label>
                <select v-model="selectedLocation" class="filter-select">
                  <option value="">เลือกทำเล</option>
                  <option v-for="loc in locations" :key="loc" :value="loc">
                    {{ loc }}
                  </option>
                </select>
              </div>

              <div class="filter-group">
                <label class="filter-label">ช่วงราคา</label>
                <select v-model="selectedPrice" class="filter-select">
                  <option value="">เลือกช่วงราคา</option>
                  <option
                    v-for="price in priceRanges"
                    :key="price.value"
                    :value="price.value"
                  >
                    {{ price.label }}
                  </option>
                </select>
              </div>

              <div class="filter-group">
                <label class="filter-label">ประเภท</label>
                <select v-model="selectedType" class="filter-select">
                  <option value="">ทุกประเภท</option>
                  <option value="condo">คอนโดมิเนียม</option>
                  <option value="house">บ้านเดี่ยว</option>
                  <option value="townhouse">ทาวน์เฮ้าส์</option>
                  <option value="land">ที่ดิน</option>
                </select>
              </div>
            </div>

            <!-- Search Button -->
            <button class="modal-search-btn" @click="performSearch">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <circle cx="11" cy="11" r="8" />
                <path d="m21 21-4.35-4.35" />
              </svg>
              ค้นหา
            </button>

            <!-- Quick Links -->
            <div class="quick-links">
              <span class="quick-label">ค้นหายอดนิยม:</span>
              <div class="quick-tags">
                <button
                  class="quick-tag"
                  @click="searchQuery = 'คอนโดสุขุมวิท'"
                >
                  คอนโดสุขุมวิท
                </button>
                <button
                  class="quick-tag"
                  @click="searchQuery = 'บ้านเดี่ยวรามอินทรา'"
                >
                  บ้านเดี่ยวรามอินทรา
                </button>
                <button
                  class="quick-tag"
                  @click="searchQuery = 'ทาวน์เฮ้าส์บางนา'"
                >
                  ทาวน์เฮ้าส์บางนา
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </Teleport>
  </header>
</template>

<script setup>
import { ref, nextTick, watch, computed, onMounted, onUnmounted } from "vue";

// Language & Currency
const currentLang = ref("th");
const isLangDropdownOpen = ref(false);

const languages = [
  { code: "th", name: "ไทย", flag: "🇹🇭", currency: "THB", currencyName: "บาท" },
  {
    code: "en",
    name: "English",
    flag: "🇺🇸",
    currency: "USD",
    currencyName: "US Dollar",
  },
  {
    code: "jp",
    name: "日本語",
    flag: "🇯🇵",
    currency: "JPY",
    currencyName: "円 (Yen)",
  },
  {
    code: "cn",
    name: "中文",
    flag: "🇨🇳",
    currency: "CNY",
    currencyName: "元 (Yuan)",
  },
  {
    code: "mm",
    name: "မြန်မာ",
    flag: "🇲🇲",
    currency: "MMK",
    currencyName: "ကျပ် (Kyat)",
  },
  {
    code: "la",
    name: "ລາວ",
    flag: "🇱🇦",
    currency: "LAK",
    currencyName: "ກີບ (Kip)",
  },
];

const currentLanguage = computed(() => {
  return (
    languages.find((lang) => lang.code === currentLang.value) || languages[0]
  );
});

const toggleLangDropdown = () => {
  isLangDropdownOpen.value = !isLangDropdownOpen.value;
};

const selectLang = (code) => {
  currentLang.value = code;
  isLangDropdownOpen.value = false;
};

// Close dropdown when clicking outside
const closeLangDropdown = (e) => {
  if (!e.target.closest(".lang-dropdown")) {
    isLangDropdownOpen.value = false;
  }
};

onMounted(() => {
  if (typeof window !== "undefined") {
    document.addEventListener("click", closeLangDropdown);
  }
});

onUnmounted(() => {
  if (typeof window !== "undefined") {
    document.removeEventListener("click", closeLangDropdown);
  }
});

// Search
const searchQuery = ref("");
const selectedLocation = ref("");
const selectedPrice = ref("");
const selectedType = ref("");
const searchInputRef = ref(null);

const locations = [
  "กรุงเทพมหานคร",
  "นนทบุรี",
  "ปทุมธานี",
  "สมุทรปราการ",
  "ชลบุรี",
  "ภูเก็ต",
  "เชียงใหม่",
];

const priceRanges = [
  { value: "0-1", label: "ไม่เกิน 1 ล้าน" },
  { value: "1-3", label: "1 - 3 ล้าน" },
  { value: "3-5", label: "3 - 5 ล้าน" },
  { value: "5-10", label: "5 - 10 ล้าน" },
  { value: "10+", label: "10 ล้านขึ้นไป" },
];

// Menu Items
const menuItems = [
  {
    name: "ซื้อ",
    link: "/buy",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>',
  },
  {
    name: "ขาย",
    link: "/sell",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="M16 8h-6a2 2 0 1 0 0 4h4a2 2 0 1 1 0 4H8"/><path d="M12 18V6"/></svg>',
  },
  {
    name: "เช่า",
    link: "/rent",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect width="18" height="18" x="3" y="4" rx="2" ry="2"/><line x1="16" x2="16" y1="2" y2="6"/><line x1="8" x2="8" y1="2" y2="6"/><line x1="3" x2="21" y1="10" y2="10"/></svg>',
  },
  {
    name: "จำนอง",
    link: "/mortgage",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 12V7H5a2 2 0 0 1 0-4h14v4"/><path d="M3 5v14a2 2 0 0 0 2 2h16v-5"/><path d="M18 12a2 2 0 0 0 0 4h4v-4Z"/></svg>',
  },
  {
    name: "ขายฝาก",
    link: "/sale-redemption",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="16" x2="8" y1="13" y2="13"/><line x1="16" x2="8" y1="17" y2="17"/></svg>',
  },
  {
    name: "ที่ปรึกษาด้านสินเชื่อ",
    link: "/loan-consultant",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M22 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>',
  },
  {
    name: "บริการช่างครบวงจร",
    link: "/services",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"/></svg>',
  },
  {
    name: "ติดต่อเรา",
    link: "/contact",
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72"/></svg>',
  },
];

// Mobile Menu
const isMobileMenuOpen = ref(false);
const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value;
};

// Search Modal
const isSearchModalOpen = ref(false);

const openSearchModal = () => {
  isSearchModalOpen.value = true;
  isMobileMenuOpen.value = false;
  document.body.style.overflow = "hidden";
  nextTick(() => {
    if (searchInputRef.value) {
      searchInputRef.value.focus();
    }
  });
};

const closeSearchModal = () => {
  isSearchModalOpen.value = false;
  document.body.style.overflow = "";
};

const performSearch = () => {
  console.log("Search:", {
    query: searchQuery.value,
    location: selectedLocation.value,
    price: selectedPrice.value,
    type: selectedType.value,
  });
  closeSearchModal();
};

// Close modal on escape key
if (typeof window !== "undefined") {
  window.addEventListener("keydown", (e) => {
    if (e.key === "Escape" && isSearchModalOpen.value) {
      closeSearchModal();
    }
  });
}
</script>

<style scoped>
/* Color Variables */
:root {
  --gold-primary: #d4af37;
  --gold-light: #f4e5b2;
  --gold-dark: #b8860b;
  --cream: #fffef7;
  --cream-dark: #f5f0e1;
  --white: #ffffff;
  --text-dark: #2c2c2c;
  --text-gold: #8b7d3a;
}

/* Header Sticky */
.header {
  position: sticky;
  top: 0;
  z-index: 1000;
}

/* Top Bar */
.top-bar {
  background: linear-gradient(135deg, #1a1a1a 0%, #2a2a2a 100%);
  padding: 8px 0;
  border-bottom: 1px solid rgba(212, 175, 55, 0.3);
}

.top-bar-container {
  max-width: 1600px;
  margin: 0 auto;
  padding: 0 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.welcome-text {
  color: #d4af37;
  font-size: 0.85rem;
  letter-spacing: 0.5px;
}

/* Language Dropdown */
.lang-dropdown {
  position: relative;
}

.lang-dropdown-trigger {
  display: flex;
  align-items: center;
  gap: 8px;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(212, 175, 55, 0.4);
  color: #fff;
  padding: 8px 14px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.85rem;
  transition: all 0.3s ease;
}

.lang-dropdown-trigger:hover {
  background: rgba(212, 175, 55, 0.2);
  border-color: #d4af37;
}

.lang-dropdown.open .lang-dropdown-trigger {
  background: rgba(212, 175, 55, 0.3);
  border-color: #d4af37;
}

.lang-dropdown-trigger .lang-flag {
  font-size: 1.2rem;
}

.lang-dropdown-trigger .lang-name {
  font-weight: 500;
}

.lang-dropdown-trigger .lang-currency {
  background: rgba(212, 175, 55, 0.3);
  padding: 2px 8px;
  border-radius: 4px;
  font-size: 0.75rem;
  font-weight: 600;
  color: #f4e5b2;
}

.dropdown-arrow {
  width: 14px;
  height: 14px;
  transition: transform 0.3s ease;
}

.lang-dropdown.open .dropdown-arrow {
  transform: rotate(180deg);
}

.lang-dropdown-menu {
  position: absolute;
  top: calc(100% + 8px);
  right: 0;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
  min-width: 280px;
  padding: 8px;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-10px);
  transition: all 0.3s ease;
  z-index: 1000;
  border: 1px solid rgba(212, 175, 55, 0.2);
}

.lang-dropdown.open .lang-dropdown-menu {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.lang-dropdown-item {
  display: flex;
  align-items: center;
  gap: 12px;
  width: 100%;
  padding: 12px 14px;
  background: transparent;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
  text-align: left;
}

.lang-dropdown-item:hover {
  background: rgba(212, 175, 55, 0.1);
}

.lang-dropdown-item.active {
  background: linear-gradient(
    135deg,
    rgba(212, 175, 55, 0.15) 0%,
    rgba(212, 175, 55, 0.08) 100%
  );
}

.lang-dropdown-item .lang-flag {
  font-size: 1.5rem;
}

.lang-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.lang-info .lang-name {
  font-size: 0.95rem;
  font-weight: 600;
  color: #2c2c2c;
}

.lang-currency-name {
  font-size: 0.8rem;
  color: #8b7d3a;
}

.lang-currency-code {
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
  color: #fff;
  padding: 4px 10px;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 700;
}

.lang-check {
  width: 18px;
  height: 18px;
  color: #d4af37;
}

.lang-flag {
  font-size: 1rem;
}

/* Navigation Bar */
.nav-bar {
  background: linear-gradient(180deg, #fffef7 0%, #f5f0e1 100%);
  border-bottom: 3px solid;
  border-image: linear-gradient(90deg, #d4af37, #f4e5b2, #d4af37) 1;
  box-shadow: 0 2px 20px rgba(212, 175, 55, 0.15);
}

.nav-container {
  max-width: 1600px;
  margin: 0 auto;
  padding: 0 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
}

/* Logo in Nav */
.nav-logo {
  flex-shrink: 0;
}

.nav-logo .logo {
  display: block;
}

.nav-logo .logo-img {
  height: 60px;
  width: auto;
  transition: transform 0.3s ease;
}

.nav-logo .logo:hover .logo-img {
  transform: scale(1.05);
}

/* Navigation Menu */
.nav-menu {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
  gap: 5px;
  flex: 1;
  justify-content: center;
}

.nav-item {
  position: relative;
}

.nav-link {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 20px 16px;
  color: #2c2c2c;
  text-decoration: none;
  font-size: 0.9rem;
  font-weight: 500;
  letter-spacing: 0.3px;
  transition: all 0.3s ease;
  position: relative;
}

.nav-link::before {
  content: "";
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 0;
  height: 3px;
  background: linear-gradient(90deg, #d4af37, #f4e5b2);
  transition: width 0.3s ease;
}

.nav-link:hover {
  color: #d4af37;
}

.nav-link:hover::before {
  width: 80%;
}

.nav-icon {
  display: flex;
  align-items: center;
}

.nav-icon :deep(svg) {
  width: 18px;
  height: 18px;
  color: #d4af37;
}

/* Search Trigger Button */
.search-trigger {
  display: flex;
  align-items: center;
  gap: 8px;
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
  color: #fff;
  border: none;
  padding: 12px 20px;
  border-radius: 50px;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(212, 175, 55, 0.3);
}

.search-trigger svg {
  width: 18px;
  height: 18px;
}

.search-trigger:hover {
  background: linear-gradient(135deg, #b8860b 0%, #8b7d3a 100%);
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(212, 175, 55, 0.4);
}

/* Nav Contact */
.nav-contact {
  flex-shrink: 0;
}

.contact-link {
  display: flex;
  align-items: center;
  gap: 8px;
  color: #2c2c2c;
  text-decoration: none;
  font-weight: 600;
  padding: 10px 16px;
  background: rgba(212, 175, 55, 0.1);
  border-radius: 8px;
  border: 1px solid rgba(212, 175, 55, 0.3);
  transition: all 0.3s ease;
}

.contact-link svg {
  width: 20px;
  height: 20px;
  color: #d4af37;
}

.contact-link:hover {
  background: rgba(212, 175, 55, 0.2);
  border-color: #d4af37;
}

/* Mobile Toggle */
.mobile-toggle {
  display: none;
  background: transparent;
  border: none;
  cursor: pointer;
  padding: 10px;
}

.hamburger {
  display: block;
  width: 26px;
  height: 2px;
  background: #d4af37;
  position: relative;
  transition: all 0.3s ease;
}

.hamburger::before,
.hamburger::after {
  content: "";
  position: absolute;
  width: 26px;
  height: 2px;
  background: #d4af37;
  left: 0;
  transition: all 0.3s ease;
}

.hamburger::before {
  top: -8px;
}

.hamburger::after {
  top: 8px;
}

.mobile-toggle.active .hamburger {
  background: transparent;
}

.mobile-toggle.active .hamburger::before {
  transform: rotate(45deg);
  top: 0;
}

.mobile-toggle.active .hamburger::after {
  transform: rotate(-45deg);
  top: 0;
}

/* Mobile Menu */
.mobile-menu {
  display: none;
  background: linear-gradient(180deg, #fffef7 0%, #f5f0e1 100%);
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.3s ease;
}

.mobile-menu.open {
  max-height: 100vh;
  overflow-y: auto;
}

.mobile-search-trigger {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  width: calc(100% - 30px);
  margin: 15px auto;
  padding: 14px 20px;
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
  color: #fff;
  border: none;
  border-radius: 50px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  box-shadow: 0 4px 15px rgba(212, 175, 55, 0.3);
}

.mobile-search-trigger svg {
  width: 20px;
  height: 20px;
}

.mobile-nav-list {
  list-style: none;
  margin: 0;
  padding: 10px 0;
}

.mobile-nav-item {
  border-bottom: 1px solid rgba(212, 175, 55, 0.15);
}

.mobile-nav-link {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 16px 20px;
  color: #2c2c2c;
  text-decoration: none;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.mobile-nav-link:hover {
  background: rgba(212, 175, 55, 0.1);
  color: #d4af37;
  padding-left: 30px;
}

.mobile-nav-icon {
  display: flex;
  align-items: center;
}

.mobile-nav-icon :deep(svg) {
  width: 20px;
  height: 20px;
  color: #d4af37;
}

.mobile-contact {
  padding: 15px 20px;
  border-top: 1px solid rgba(212, 175, 55, 0.15);
}

.mobile-contact-link {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  color: #d4af37;
  text-decoration: none;
  font-weight: 600;
  font-size: 1.1rem;
}

.mobile-contact-link svg {
  width: 22px;
  height: 22px;
}

/* Search Modal Overlay */
.search-modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  backdrop-filter: blur(8px);
  z-index: 2000;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  padding: 5vh 20px;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.search-modal-overlay.active {
  opacity: 1;
  visibility: visible;
}

/* Search Modal */
.search-modal {
  background: linear-gradient(180deg, #fffef7 0%, #f5f0e1 100%);
  border-radius: 20px;
  width: 100%;
  max-width: 700px;
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 25px 80px rgba(212, 175, 55, 0.3);
  transform: translateY(-30px);
  transition: all 0.3s ease;
  border: 2px solid rgba(212, 175, 55, 0.3);
}

.search-modal-overlay.active .search-modal {
  transform: translateY(0);
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 25px;
  border-bottom: 1px solid rgba(212, 175, 55, 0.2);
}

.modal-logo-img {
  height: 50px;
  width: auto;
}

.modal-close {
  background: transparent;
  border: 2px solid rgba(212, 175, 55, 0.3);
  width: 44px;
  height: 44px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.modal-close svg {
  width: 20px;
  height: 20px;
  color: #2c2c2c;
}

.modal-close:hover {
  background: #d4af37;
  border-color: #d4af37;
}

.modal-close:hover svg {
  color: #fff;
}

.modal-body {
  padding: 30px 25px;
}

.modal-title {
  font-size: 1.8rem;
  font-weight: 700;
  color: #2c2c2c;
  margin: 0 0 8px 0;
  text-align: center;
}

.modal-subtitle {
  font-size: 1rem;
  color: #8b7d3a;
  text-align: center;
  margin: 0 0 25px 0;
}

.modal-search-input {
  display: flex;
  align-items: center;
  gap: 12px;
  background: #fff;
  border: 2px solid rgba(212, 175, 55, 0.3);
  border-radius: 12px;
  padding: 15px 20px;
  margin-bottom: 20px;
  transition: all 0.3s ease;
}

.modal-search-input:focus-within {
  border-color: #d4af37;
  box-shadow: 0 4px 20px rgba(212, 175, 55, 0.2);
}

.modal-search-input svg {
  width: 24px;
  height: 24px;
  color: #d4af37;
  flex-shrink: 0;
}

.modal-search-input input {
  flex: 1;
  border: none;
  outline: none;
  font-size: 1.1rem;
  color: #2c2c2c;
  background: transparent;
}

.modal-search-input input::placeholder {
  color: #999;
}

.modal-filters {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 15px;
  margin-bottom: 25px;
}

.filter-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.filter-label {
  font-size: 0.85rem;
  font-weight: 600;
  color: #8b7d3a;
}

.filter-select {
  padding: 12px 15px;
  border: 2px solid rgba(212, 175, 55, 0.3);
  border-radius: 10px;
  font-size: 0.95rem;
  color: #2c2c2c;
  background: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
}

.filter-select:focus {
  outline: none;
  border-color: #d4af37;
}

.modal-search-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  width: 100%;
  padding: 16px;
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
  color: #fff;
  border: none;
  border-radius: 12px;
  font-size: 1.1rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 20px rgba(212, 175, 55, 0.3);
}

.modal-search-btn svg {
  width: 22px;
  height: 22px;
}

.modal-search-btn:hover {
  background: linear-gradient(135deg, #b8860b 0%, #8b7d3a 100%);
  transform: translateY(-2px);
  box-shadow: 0 6px 25px rgba(212, 175, 55, 0.4);
}

.quick-links {
  margin-top: 25px;
  text-align: center;
}

.quick-label {
  font-size: 0.9rem;
  color: #8b7d3a;
  display: block;
  margin-bottom: 12px;
}

.quick-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
}

.quick-tag {
  background: rgba(212, 175, 55, 0.1);
  border: 1px solid rgba(212, 175, 55, 0.3);
  color: #2c2c2c;
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 0.85rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.quick-tag:hover {
  background: #d4af37;
  color: #fff;
  border-color: #d4af37;
}

/* Responsive - Large Desktop */
@media (max-width: 1399px) {
  .nav-link {
    padding: 18px 12px;
    font-size: 0.85rem;
  }

  .search-trigger-text {
    display: none;
  }

  .search-trigger {
    padding: 12px;
    border-radius: 50%;
  }
}

/* Responsive - Desktop */
@media (max-width: 1199px) {
  .nav-link {
    padding: 16px 10px;
    font-size: 0.8rem;
  }

  .nav-icon {
    display: none;
  }

  .nav-logo .logo-img {
    height: 50px;
  }

  .nav-contact {
    display: none;
  }
}

/* Responsive - Tablet */
@media (max-width: 991px) {
  .nav-menu {
    display: none;
  }

  .search-trigger {
    display: none;
  }

  .mobile-toggle {
    display: block;
  }

  .mobile-menu {
    display: block;
  }

  .nav-container {
    padding: 10px 20px;
  }

  .nav-logo .logo-img {
    height: 45px;
  }
}

/* Responsive - Mobile */
@media (max-width: 767px) {
  .top-bar {
    padding: 6px 0;
  }

  .top-bar-container {
    flex-direction: column;
    gap: 8px;
  }

  .welcome-text {
    font-size: 0.8rem;
  }

  .lang-name {
    display: none;
  }

  .nav-logo .logo-img {
    height: 40px;
  }

  .modal-filters {
    grid-template-columns: 1fr;
  }

  .modal-title {
    font-size: 1.5rem;
  }

  .modal-search-input {
    padding: 12px 15px;
  }

  .modal-search-input input {
    font-size: 1rem;
  }
}

/* Responsive - Small Mobile */
@media (max-width: 575px) {
  .top-bar {
    padding: 5px 0;
  }

  .welcome-text {
    font-size: 0.75rem;
    text-align: center;
  }

  .lang-btn {
    padding: 3px 6px;
  }

  .lang-flag {
    font-size: 0.9rem;
  }

  .nav-container {
    padding: 8px 15px;
  }

  .nav-logo .logo-img {
    height: 35px;
  }

  .mobile-toggle {
    padding: 8px;
  }

  .hamburger {
    width: 22px;
  }

  .hamburger::before,
  .hamburger::after {
    width: 22px;
  }

  .search-modal {
    border-radius: 15px;
  }

  .modal-header {
    padding: 15px 20px;
  }

  .modal-logo-img {
    height: 40px;
  }

  .modal-close {
    width: 38px;
    height: 38px;
  }

  .modal-body {
    padding: 20px;
  }

  .modal-title {
    font-size: 1.3rem;
  }

  .modal-subtitle {
    font-size: 0.9rem;
  }

  .quick-tag {
    font-size: 0.8rem;
    padding: 6px 12px;
  }
}

/* Print styles */
@media print {
  .header {
    position: relative;
  }

  .mobile-toggle,
  .mobile-menu,
  .language-switcher,
  .search-trigger,
  .search-modal-overlay {
    display: none !important;
  }
}
</style>

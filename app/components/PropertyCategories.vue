<template>
  <section class="categories-section">
    <div class="container">
      <div class="section-header">
        <span class="section-tag">ค้นหาตามประเภท</span>
        <h2 class="section-title">ประเภทอสังหาริมทรัพย์</h2>
        <p class="section-subtitle">เลือกประเภทอสังหาริมทรัพย์ที่คุณสนใจ</p>
      </div>

      <div class="categories-grid">
        <NuxtLink
          v-for="category in categories"
          :key="category.name"
          :to="category.link"
          class="category-card"
        >
          <div class="category-image">
            <img :src="category.image" :alt="category.name" />
            <div class="category-overlay"></div>
          </div>
          <div class="category-content">
            <h3 class="category-name">{{ category.name }}</h3>
            <span class="category-count">{{ category.count }} รายการ</span>
          </div>
          <div class="category-icon">
            <span v-html="category.icon"></span>
          </div>
        </NuxtLink>
      </div>
    </div>
  </section>
</template>

<script setup>
const categories = [
  {
    name: 'คอนโดมิเนียม',
    count: 1250,
    link: '/condo',
    image: 'https://images.unsplash.com/photo-1545324418-cc1a3fa10c00?w=600',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="4" y="2" width="16" height="20" rx="2" ry="2"/><path d="M9 22v-4h6v4"/><path d="M8 6h.01M16 6h.01M12 6h.01M8 10h.01M16 10h.01M12 10h.01M8 14h.01M16 14h.01M12 14h.01"/></svg>'
  },
  {
    name: 'บ้านเดี่ยว',
    count: 856,
    link: '/house',
    image: 'https://images.unsplash.com/photo-1600596542815-ffad4c1539a9?w=600',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>'
  },
  {
    name: 'ทาวน์เฮ้าส์',
    count: 432,
    link: '/townhouse',
    image: 'https://images.unsplash.com/photo-1600585154340-be6161a56a0c?w=600',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 21h18"/><path d="M5 21V7l8-4v18"/><path d="M19 21V11l-6-4"/><path d="M9 9v.01M9 12v.01M9 15v.01M9 18v.01"/></svg>'
  },
  {
    name: 'ที่ดิน',
    count: 298,
    link: '/land',
    image: 'https://images.unsplash.com/photo-1500382017468-9049fed747ef?w=600',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"/></svg>'
  },
  {
    name: 'อาคารพาณิชย์',
    count: 167,
    link: '/commercial',
    image: 'https://images.unsplash.com/photo-1486406146926-c627a92ad1ab?w=600',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M6 22V4a2 2 0 0 1 2-2h8a2 2 0 0 1 2 2v18Z"/><path d="M6 12H4a2 2 0 0 0-2 2v6a2 2 0 0 0 2 2h2"/><path d="M18 9h2a2 2 0 0 1 2 2v9a2 2 0 0 1-2 2h-2"/><path d="M10 6h4"/><path d="M10 10h4"/><path d="M10 14h4"/><path d="M10 18h4"/></svg>'
  },
  {
    name: 'โรงแรม/รีสอร์ท',
    count: 45,
    link: '/hotel',
    image: 'https://images.unsplash.com/photo-1566073771259-6a8506099945?w=600',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V4a2 2 0 0 0-2-2Z"/><path d="m9 16 .348-.24c1.465-1.013 3.84-1.013 5.304 0L15 16"/><path d="M8 7h.01M16 7h.01M12 7h.01M8 11h.01M16 11h.01M12 11h.01"/></svg>'
  }
];
</script>

<style scoped>
.categories-section {
  padding: 80px 0;
  background: linear-gradient(180deg, #fffef7 0%, #f8f4e8 100%);
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 20px;
}

.section-header {
  text-align: center;
  margin-bottom: 50px;
}

.section-tag {
  display: inline-block;
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  color: #fff;
  padding: 8px 20px;
  border-radius: 30px;
  font-size: 0.85rem;
  font-weight: 600;
  margin-bottom: 15px;
  letter-spacing: 0.5px;
}

.section-title {
  font-size: 2.5rem;
  font-weight: 700;
  color: #1a1a1a;
  margin: 0 0 15px 0;
}

.section-subtitle {
  font-size: 1.1rem;
  color: #666;
  margin: 0;
}

.categories-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 25px;
}

.category-card {
  position: relative;
  border-radius: 20px;
  overflow: hidden;
  aspect-ratio: 16/10;
  text-decoration: none;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.1);
  transition: all 0.4s ease;
}

.category-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 50px rgba(212, 175, 55, 0.2);
}

.category-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.category-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.category-card:hover .category-image img {
  transform: scale(1.1);
}

.category-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    to top,
    rgba(0, 0, 0, 0.8) 0%,
    rgba(0, 0, 0, 0.3) 50%,
    rgba(0, 0, 0, 0.1) 100%
  );
  transition: all 0.3s ease;
}

.category-card:hover .category-overlay {
  background: linear-gradient(
    to top,
    rgba(212, 175, 55, 0.9) 0%,
    rgba(212, 175, 55, 0.3) 50%,
    rgba(0, 0, 0, 0.1) 100%
  );
}

.category-content {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 25px;
  z-index: 2;
}

.category-name {
  font-size: 1.4rem;
  font-weight: 700;
  color: #fff;
  margin: 0 0 5px 0;
}

.category-count {
  font-size: 0.9rem;
  color: rgba(255, 255, 255, 0.8);
}

.category-icon {
  position: absolute;
  top: 20px;
  right: 20px;
  width: 50px;
  height: 50px;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.category-card:hover .category-icon {
  background: #DAA520;
}

.category-icon :deep(svg) {
  width: 24px;
  height: 24px;
  color: #fff;
}

/* Responsive */
@media (max-width: 991px) {
  .categories-section {
    padding: 60px 0;
  }
  
  .section-title {
    font-size: 2rem;
  }
  
  .categories-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
}

@media (max-width: 575px) {
  .categories-section {
    padding: 50px 0;
  }
  
  .section-title {
    font-size: 1.6rem;
  }
  
  .section-subtitle {
    font-size: 1rem;
  }
  
  .categories-grid {
    grid-template-columns: 1fr;
    gap: 15px;
  }
  
  .category-card {
    aspect-ratio: 16/9;
  }
  
  .category-name {
    font-size: 1.2rem;
  }
}
</style>

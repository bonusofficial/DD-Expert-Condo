<template>
  <section class="featured-section">
    <div class="container">
      <div class="section-header">
        <div class="header-left">
          <span class="section-tag">อสังหาฯ แนะนำ</span>
          <h2 class="section-title">คอนโดมิเนียมแนะนำ</h2>
        </div>
        <NuxtLink to="/condo" class="view-all-btn">
          ดูทั้งหมด
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M5 12h14M12 5l7 7-7 7"/>
          </svg>
        </NuxtLink>
      </div>

      <div class="properties-grid">
        <article
          v-for="property in properties"
          :key="property.id"
          class="property-card"
        >
          <!-- Image Section -->
          <div class="property-image">
            <img :src="property.image" :alt="property.title" />
            <div class="property-badges">
              <span v-if="property.isNew" class="badge badge-new">ใหม่</span>
              <span v-if="property.isHot" class="badge badge-hot">ยอดนิยม</span>
              <span v-if="property.discount" class="badge badge-discount">-{{ property.discount }}%</span>
            </div>
            <button class="favorite-btn" @click.prevent="toggleFavorite(property.id)">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/>
              </svg>
            </button>
            <div class="property-type">{{ property.type }}</div>
          </div>

          <!-- Content Section -->
          <div class="property-content">
            <div class="property-price">
              <span class="price-current">฿{{ formatPrice(property.price) }}</span>
              <span v-if="property.pricePerSqm" class="price-sqm">฿{{ formatPrice(property.pricePerSqm) }}/ตร.ม.</span>
            </div>

            <h3 class="property-title">{{ property.title }}</h3>
            <p class="property-location">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
                <circle cx="12" cy="10" r="3"/>
              </svg>
              {{ property.location }}
            </p>

            <div class="property-specs">
              <div class="spec-item" v-if="property.bedrooms">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M2 4v16"/><path d="M2 8h18a2 2 0 0 1 2 2v10"/><path d="M2 17h20"/><path d="M6 8v9"/>
                </svg>
                <span>{{ property.bedrooms }} ห้องนอน</span>
              </div>
              <div class="spec-item" v-if="property.bathrooms">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M9 6 6.5 3.5a1.5 1.5 0 0 0-1-.5C4.683 3 4 3.683 4 4.5V17a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2v-5"/><line x1="10" x2="8" y1="5" y2="7"/><line x1="2" x2="22" y1="12" y2="12"/><line x1="7" x2="7" y1="19" y2="21"/><line x1="17" x2="17" y1="19" y2="21"/>
                </svg>
                <span>{{ property.bathrooms }} ห้องน้ำ</span>
              </div>
              <div class="spec-item" v-if="property.area">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <rect width="18" height="18" x="3" y="3" rx="2" ry="2"/>
                </svg>
                <span>{{ property.area }} ตร.ม.</span>
              </div>
            </div>

            <div class="property-footer">
              <div class="property-agent">
                <img :src="property.agent.avatar" :alt="property.agent.name" class="agent-avatar" />
                <span class="agent-name">{{ property.agent.name }}</span>
              </div>
              <NuxtLink :to="`/property/${property.id}`" class="property-btn">
                ดูรายละเอียด
              </NuxtLink>
            </div>
          </div>
        </article>
      </div>
    </div>
  </section>
</template>

<script setup>
const props = defineProps({
  title: {
    type: String,
    default: 'คอนโดมิเนียมแนะนำ'
  },
  link: {
    type: String,
    default: '/condo'
  }
});

const properties = [
  {
    id: 1,
    title: 'Ideo Q สุขุมวิท 36',
    type: 'คอนโด',
    price: 4990000,
    pricePerSqm: 185000,
    location: 'สุขุมวิท, กรุงเทพฯ',
    bedrooms: 1,
    bathrooms: 1,
    area: 35,
    image: 'https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=600',
    isNew: true,
    isHot: false,
    discount: null,
    agent: {
      name: 'คุณนภา',
      avatar: 'https://randomuser.me/api/portraits/women/44.jpg'
    }
  },
  {
    id: 2,
    title: 'The Line พหลโยธิน',
    type: 'คอนโด',
    price: 6500000,
    pricePerSqm: 210000,
    location: 'พหลโยธิน, กรุงเทพฯ',
    bedrooms: 2,
    bathrooms: 2,
    area: 55,
    image: 'https://images.unsplash.com/photo-1560448204-e02f11c3d0e2?w=600',
    isNew: false,
    isHot: true,
    discount: 5,
    agent: {
      name: 'คุณสมชาย',
      avatar: 'https://randomuser.me/api/portraits/men/32.jpg'
    }
  },
  {
    id: 3,
    title: 'Noble Ploenchit',
    type: 'คอนโด',
    price: 12500000,
    pricePerSqm: 280000,
    location: 'เพลินจิต, กรุงเทพฯ',
    bedrooms: 2,
    bathrooms: 2,
    area: 65,
    image: 'https://images.unsplash.com/photo-1600607687939-ce8a6c25118c?w=600',
    isNew: true,
    isHot: true,
    discount: null,
    agent: {
      name: 'คุณวิภา',
      avatar: 'https://randomuser.me/api/portraits/women/68.jpg'
    }
  },
  {
    id: 4,
    title: 'Life Ladprao Valley',
    type: 'คอนโด',
    price: 3290000,
    pricePerSqm: 145000,
    location: 'ลาดพร้าว, กรุงเทพฯ',
    bedrooms: 1,
    bathrooms: 1,
    area: 28,
    image: 'https://images.unsplash.com/photo-1600566753190-17f0baa2a6c3?w=600',
    isNew: false,
    isHot: false,
    discount: 10,
    agent: {
      name: 'คุณธนา',
      avatar: 'https://randomuser.me/api/portraits/men/75.jpg'
    }
  }
];

const formatPrice = (price) => {
  return new Intl.NumberFormat('th-TH').format(price);
};

const toggleFavorite = (id) => {
  console.log('Toggle favorite:', id);
};
</script>

<style scoped>
.featured-section {
  padding: 80px 0;
  background: #fff;
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 20px;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 40px;
}

.header-left {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.section-tag {
  display: inline-block;
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
  color: #fff;
  padding: 6px 16px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 600;
  width: fit-content;
}

.section-title {
  font-size: 2rem;
  font-weight: 700;
  color: #1a1a1a;
  margin: 0;
}

.view-all-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  color: #d4af37;
  text-decoration: none;
  font-weight: 600;
  font-size: 0.95rem;
  transition: all 0.3s ease;
}

.view-all-btn svg {
  width: 18px;
  height: 18px;
  transition: transform 0.3s ease;
}

.view-all-btn:hover {
  color: #b8860b;
}

.view-all-btn:hover svg {
  transform: translateX(5px);
}

.properties-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 25px;
}

.property-card {
  background: #fff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 5px 20px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
}

.property-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 15px 40px rgba(212, 175, 55, 0.15);
}

.property-image {
  position: relative;
  aspect-ratio: 4/3;
  overflow: hidden;
}

.property-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.property-card:hover .property-image img {
  transform: scale(1.1);
}

.property-badges {
  position: absolute;
  top: 12px;
  left: 12px;
  display: flex;
  gap: 6px;
}

.badge {
  padding: 4px 10px;
  border-radius: 4px;
  font-size: 0.7rem;
  font-weight: 600;
  text-transform: uppercase;
}

.badge-new {
  background: #d4af37;
  color: #fff;
}

.badge-hot {
  background: #ef4444;
  color: #fff;
}

.badge-discount {
  background: #22c55e;
  color: #fff;
}

.favorite-btn {
  position: absolute;
  top: 12px;
  right: 12px;
  width: 36px;
  height: 36px;
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
  width: 18px;
  height: 18px;
  color: #999;
  transition: all 0.3s ease;
}

.favorite-btn:hover {
  background: #ef4444;
}

.favorite-btn:hover svg {
  color: #fff;
  fill: #fff;
}

.property-type {
  position: absolute;
  bottom: 12px;
  left: 12px;
  background: rgba(0, 0, 0, 0.7);
  color: #fff;
  padding: 4px 12px;
  border-radius: 4px;
  font-size: 0.75rem;
  font-weight: 500;
}

.property-content {
  padding: 20px;
}

.property-price {
  display: flex;
  align-items: baseline;
  gap: 10px;
  margin-bottom: 10px;
}

.price-current {
  font-size: 1.3rem;
  font-weight: 700;
  color: #d4af37;
}

.price-sqm {
  font-size: 0.8rem;
  color: #999;
}

.property-title {
  font-size: 1.05rem;
  font-weight: 600;
  color: #1a1a1a;
  margin: 0 0 8px 0;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  line-clamp: 1;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.property-location {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #666;
  font-size: 0.85rem;
  margin: 0 0 15px 0;
}

.property-location svg {
  width: 14px;
  height: 14px;
  color: #d4af37;
}

.property-specs {
  display: flex;
  gap: 15px;
  padding: 15px 0;
  border-top: 1px solid #eee;
  border-bottom: 1px solid #eee;
  margin-bottom: 15px;
}

.spec-item {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 0.8rem;
  color: #666;
}

.spec-item svg {
  width: 14px;
  height: 14px;
  color: #999;
}

.property-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.property-agent {
  display: flex;
  align-items: center;
  gap: 8px;
}

.agent-avatar {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  object-fit: cover;
}

.agent-name {
  font-size: 0.8rem;
  color: #666;
}

.property-btn {
  background: linear-gradient(135deg, #1a1a1a 0%, #333 100%);
  color: #fff;
  padding: 8px 16px;
  border-radius: 6px;
  font-size: 0.8rem;
  font-weight: 500;
  text-decoration: none;
  transition: all 0.3s ease;
}

.property-btn:hover {
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
}

/* Responsive */
@media (max-width: 1199px) {
  .properties-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 991px) {
  .featured-section {
    padding: 60px 0;
  }
  
  .properties-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
}

@media (max-width: 767px) {
  .section-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 15px;
  }
  
  .section-title {
    font-size: 1.6rem;
  }
}

@media (max-width: 575px) {
  .featured-section {
    padding: 50px 0;
  }
  
  .properties-grid {
    grid-template-columns: 1fr;
  }
  
  .property-specs {
    flex-wrap: wrap;
    gap: 10px;
  }
}
</style>

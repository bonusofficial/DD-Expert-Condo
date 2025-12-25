<template>
  <section class="ads-section">
    <div class="container">
      <div class="ads-grid">
        <!-- Main Ad Banner -->
        <div class="ad-main">
          <div class="ad-content">
            <span class="ad-label">โฆษณา</span>
            <h3 class="ad-title">ลงโฆษณากับเรา</h3>
            <p class="ad-description">เข้าถึงกลุ่มลูกค้าที่กำลังมองหาอสังหาริมทรัพย์กว่า 100,000 คนต่อเดือน</p>
            <a href="/advertise" class="ad-btn">สนใจลงโฆษณา</a>
          </div>
          <div class="ad-image">
            <img src="https://images.unsplash.com/photo-1460925895917-afdab827c52f?w=600" alt="Advertise with us" />
          </div>
        </div>

        <!-- Side Ads -->
        <div class="ad-side">
          <a href="#" class="side-ad-item" target="_blank">
            <img src="https://images.unsplash.com/photo-1545324418-cc1a3fa10c00?w=400" alt="Ad 1" />
            <div class="ad-overlay">
              <span>คลิกเพื่อดูรายละเอียด</span>
            </div>
          </a>
          <a href="#" class="side-ad-item" target="_blank">
            <img src="https://images.unsplash.com/photo-1600596542815-ffad4c1539a9?w=400" alt="Ad 2" />
            <div class="ad-overlay">
              <span>คลิกเพื่อดูรายละเอียด</span>
            </div>
          </a>
        </div>
      </div>

      <!-- Website Partners -->
      <div class="partners-grid">
        <h4 class="partners-title">Website Partners</h4>
        <div class="partners-list">
          <a
            v-for="partner in partners"
            :key="partner.name"
            :href="partner.url"
            target="_blank"
            class="partner-link"
          >
            <img :src="partner.logo" :alt="partner.name" />
            <span>{{ partner.name }}</span>
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
const partners = [
  { name: 'DDProperty', url: 'https://ddproperty.com', logo: 'https://logo.clearbit.com/ddproperty.com' },
  { name: 'Thinkofliving', url: 'https://thinkofliving.com', logo: 'https://logo.clearbit.com/thinkofliving.com' },
  { name: 'Baania', url: 'https://baania.com', logo: 'https://logo.clearbit.com/baania.com' },
  { name: 'Hipflat', url: 'https://hipflat.co.th', logo: 'https://logo.clearbit.com/hipflat.co.th' },
  { name: 'Livinginsider', url: 'https://livinginsider.com', logo: 'https://logo.clearbit.com/livinginsider.com' },
];
</script>

<style scoped>
.ads-section {
  padding: 60px 0;
  background: linear-gradient(180deg, #f8f4e8 0%, #fffef7 100%);
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 20px;
}

.ads-grid {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 25px;
  margin-bottom: 50px;
}

/* Main Ad */
.ad-main {
  background: linear-gradient(135deg, #1a1a1a 0%, #2a2a2a 100%);
  border-radius: 20px;
  overflow: hidden;
  display: grid;
  grid-template-columns: 1fr 1fr;
  min-height: 280px;
}

.ad-content {
  padding: 40px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.ad-label {
  display: inline-block;
  background: rgba(212, 175, 55, 0.2);
  color: #DAA520;
  padding: 4px 12px;
  border-radius: 4px;
  font-size: 0.75rem;
  font-weight: 600;
  margin-bottom: 15px;
  width: fit-content;
}

.ad-title {
  font-size: 1.8rem;
  font-weight: 700;
  color: #fff;
  margin: 0 0 12px 0;
}

.ad-description {
  font-size: 0.95rem;
  color: rgba(255, 255, 255, 0.7);
  line-height: 1.6;
  margin: 0 0 25px 0;
}

.ad-btn {
  display: inline-flex;
  align-items: center;
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  color: #fff;
  padding: 12px 28px;
  border-radius: 50px;
  font-size: 0.95rem;
  font-weight: 600;
  text-decoration: none;
  width: fit-content;
  transition: all 0.3s ease;
}

.ad-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 25px rgba(212, 175, 55, 0.4);
}

.ad-image {
  position: relative;
  overflow: hidden;
}

.ad-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* Side Ads */
.ad-side {
  display: flex;
  flex-direction: column;
  gap: 25px;
}

.side-ad-item {
  position: relative;
  border-radius: 16px;
  overflow: hidden;
  flex: 1;
  display: block;
}

.side-ad-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.side-ad-item:hover img {
  transform: scale(1.1);
}

.ad-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.side-ad-item:hover .ad-overlay {
  opacity: 1;
}

.ad-overlay span {
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  color: #fff;
  padding: 10px 20px;
  border-radius: 30px;
  font-size: 0.85rem;
  font-weight: 600;
}

/* Partners */
.partners-grid {
  background: #fff;
  border-radius: 20px;
  padding: 30px 40px;
  box-shadow: 0 5px 30px rgba(0, 0, 0, 0.05);
}

.partners-title {
  font-size: 1rem;
  font-weight: 600;
  color: #999;
  text-align: center;
  margin: 0 0 25px 0;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.partners-list {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  gap: 40px;
}

.partner-link {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  text-decoration: none;
  transition: all 0.3s ease;
  filter: grayscale(100%);
  opacity: 0.6;
}

.partner-link:hover {
  filter: grayscale(0%);
  opacity: 1;
  transform: translateY(-3px);
}

.partner-link img {
  height: 40px;
  width: auto;
  max-width: 100px;
  object-fit: contain;
}

.partner-link span {
  font-size: 0.8rem;
  color: #666;
}

/* Responsive */
@media (max-width: 991px) {
  .ads-grid {
    grid-template-columns: 1fr;
  }
  
  .ad-side {
    flex-direction: row;
  }
  
  .side-ad-item {
    height: 200px;
  }
}

@media (max-width: 767px) {
  .ad-main {
    grid-template-columns: 1fr;
  }
  
  .ad-image {
    height: 200px;
  }
  
  .ad-content {
    padding: 30px;
  }
  
  .ad-title {
    font-size: 1.4rem;
  }
  
  .ad-side {
    flex-direction: column;
  }
  
  .partners-list {
    gap: 25px;
  }
  
  .partner-link img {
    height: 30px;
  }
}

@media (max-width: 575px) {
  .ads-section {
    padding: 40px 0;
  }
  
  .partners-grid {
    padding: 20px;
  }
  
  .partners-list {
    gap: 20px;
  }
}
</style>

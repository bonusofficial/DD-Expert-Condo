<template>
  <section class="partners-section">
    <div class="container">
      <div class="section-header">
        <h3 class="partners-title">พันธมิตรของเรา</h3>
        <p class="partners-subtitle">ร่วมมือกับ Developer ชั้นนำ</p>
      </div>

      <div class="partners-slider">
        <div class="partners-track">
          <div
            v-for="(partner, index) in [...partners, ...partners]"
            :key="index"
            class="partner-item"
          >
            <img :src="partner.logo" :alt="partner.name" />
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
const partners = [
  { name: 'Sansiri', logo: 'https://logo.clearbit.com/sansiri.com' },
  { name: 'AP Thailand', logo: 'https://logo.clearbit.com/apthai.com' },
  { name: 'Ananda', logo: 'https://logo.clearbit.com/ananda.co.th' },
  { name: 'Origin', logo: 'https://logo.clearbit.com/origin.co.th' },
  { name: 'LPN', logo: 'https://logo.clearbit.com/lpn.co.th' },
  { name: 'Pruksa', logo: 'https://logo.clearbit.com/pruksa.com' },
];
</script>

<style scoped>
.partners-section {
  padding: 50px 0;
  background: #fff;
  border-top: 1px solid #eee;
  border-bottom: 1px solid #eee;
  overflow: hidden;
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

.partners-title {
  font-size: 1.3rem;
  font-weight: 600;
  color: #1a1a1a;
  margin: 0 0 8px 0;
}

.partners-subtitle {
  font-size: 0.9rem;
  color: #999;
  margin: 0;
}

.partners-slider {
  overflow: hidden;
  width: 100%;
}

.partners-track {
  display: flex;
  gap: 60px;
  animation: scroll 25s linear infinite;
}

.partner-item {
  flex-shrink: 0;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  filter: grayscale(100%);
  opacity: 0.6;
  transition: all 0.3s ease;
}

.partner-item:hover {
  filter: grayscale(0%);
  opacity: 1;
}

.partner-item img {
  max-height: 100%;
  max-width: 140px;
  object-fit: contain;
}

@keyframes scroll {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-50%);
  }
}

/* Responsive */
@media (max-width: 767px) {
  .partners-section {
    padding: 40px 0;
  }
  
  .partners-track {
    gap: 40px;
  }
  
  .partner-item {
    height: 40px;
  }
  
  .partner-item img {
    max-width: 100px;
  }
}
</style>

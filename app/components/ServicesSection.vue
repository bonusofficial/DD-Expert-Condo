<template>
  <section class="services-section">
    <div class="container">
      <div class="section-header">
        <span class="section-tag">ทำไมต้องเลือกเรา</span>
        <h2 class="section-title">บริการของเรา</h2>
        <p class="section-subtitle">DD Expert Condo พร้อมให้บริการครบวงจร</p>
      </div>

      <div class="services-grid">
        <div v-for="service in services" :key="service.title" class="service-card">
          <div class="service-icon" :style="{ background: service.color }">
            <span v-html="service.icon"></span>
          </div>
          <h3 class="service-title">{{ service.title }}</h3>
          <p class="service-description">{{ service.description }}</p>
          <div class="service-stats" v-if="service.stats">
            <div class="stat-value">{{ service.stats.value }}</div>
            <div class="stat-label">{{ service.stats.label }}</div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
const services = [
  {
    title: 'ทีมงานมืออาชีพ',
    description: 'ทีมงานที่มีประสบการณ์มากกว่า 10 ปี พร้อมให้คำปรึกษาอย่างใกล้ชิด',
    color: 'linear-gradient(135deg, #d4af37 0%, #b8860b 100%)',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M22 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>',
    stats: { value: '50+', label: 'ทีมงานผู้เชี่ยวชาญ' }
  },
  {
    title: 'VR Tour 360°',
    description: 'ชมโครงการแบบ Virtual Reality ได้ทุกที่ทุกเวลา ไม่ต้องเดินทาง',
    color: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="M12 2a14.5 14.5 0 0 0 0 20 14.5 14.5 0 0 0 0-20"/><path d="M2 12h20"/></svg>',
    stats: { value: '100%', label: 'VR Tour ทุกโครงการ' }
  },
  {
    title: 'รับประกันความปลอดภัย',
    description: 'ตรวจสอบทุกขั้นตอนอย่างรอบคอบ มั่นใจได้ในทุกการทำธุรกรรม',
    color: 'linear-gradient(135deg, #11998e 0%, #38ef7d 100%)',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/><path d="m9 12 2 2 4-4"/></svg>',
    stats: { value: '100%', label: 'การันตีความปลอดภัย' }
  },
  {
    title: 'บริการหลังการขาย',
    description: 'ดูแลลูกค้าไม่ใช่แค่การซื้อขาย แต่รวมถึงบริการช่างครบวงจร',
    color: 'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)',
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"/></svg>',
    stats: { value: '24/7', label: 'พร้อมให้บริการ' }
  }
];
</script>

<style scoped>
.services-section {
  padding: 80px 0;
  background: linear-gradient(180deg, #f8f4e8 0%, #fffef7 100%);
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
  color: #1a1a1a;
  margin: 0 0 15px 0;
}

.section-subtitle {
  font-size: 1.1rem;
  color: #666;
  margin: 0;
}

.services-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 30px;
}

.service-card {
  background: #fff;
  border-radius: 20px;
  padding: 35px 25px;
  text-align: center;
  box-shadow: 0 5px 30px rgba(0, 0, 0, 0.06);
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.service-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, #d4af37, #f4e5b2, #d4af37);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.service-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 15px 50px rgba(212, 175, 55, 0.15);
}

.service-card:hover::before {
  opacity: 1;
}

.service-icon {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 25px;
}

.service-icon :deep(svg) {
  width: 36px;
  height: 36px;
  color: #fff;
}

.service-title {
  font-size: 1.2rem;
  font-weight: 700;
  color: #1a1a1a;
  margin: 0 0 12px 0;
}

.service-description {
  font-size: 0.9rem;
  color: #666;
  line-height: 1.7;
  margin: 0 0 20px 0;
}

.service-stats {
  padding-top: 20px;
  border-top: 1px dashed #eee;
}

.stat-value {
  font-size: 2rem;
  font-weight: 800;
  color: #d4af37;
  line-height: 1;
}

.stat-label {
  font-size: 0.8rem;
  color: #999;
  margin-top: 5px;
}

/* Responsive */
@media (max-width: 1199px) {
  .services-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 767px) {
  .services-section {
    padding: 60px 0;
  }
  
  .section-title {
    font-size: 2rem;
  }
  
  .services-grid {
    gap: 20px;
  }
  
  .service-card {
    padding: 25px 20px;
  }
}

@media (max-width: 575px) {
  .services-grid {
    grid-template-columns: 1fr;
  }
  
  .section-title {
    font-size: 1.6rem;
  }
}
</style>

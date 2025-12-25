<template>
  <section class="announcements-section">
    <div class="container">
      <div class="section-header">
        <span class="section-tag">ร่วมงานกับเรา</span>
        <h2 class="section-title">ประกาศรับสมัคร</h2>
      </div>

      <div class="announcements-grid">
        <!-- Team Recruitment -->
        <div class="announcement-card team">
          <div class="card-icon">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/>
              <circle cx="9" cy="7" r="4"/>
              <path d="M22 21v-2a4 4 0 0 0-3-3.87"/>
              <path d="M16 3.13a4 4 0 0 1 0 7.75"/>
            </svg>
          </div>
          <div class="card-content">
            <h3 class="card-title">รับสมัครทีมงาน</h3>
            <p class="card-description">
              เรากำลังมองหาคนรุ่นใหม่ที่มีความกระตือรือร้น พร้อมเติบโตไปด้วยกันกับทีม DD Expert
            </p>
            <ul class="position-list">
              <li>Marketing Executive</li>
              <li>Content Creator</li>
              <li>Customer Service</li>
            </ul>
          </div>
          <NuxtLink to="/careers" class="card-btn">
            ดูตำแหน่งงาน
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M5 12h14M12 5l7 7-7 7"/>
            </svg>
          </NuxtLink>
        </div>

        <!-- Agent Recruitment -->
        <div class="announcement-card agent">
          <div class="card-badge">ผลตอบแทนสูง</div>
          <div class="card-icon">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="12" cy="12" r="10"/>
              <path d="M16 8h-6a2 2 0 1 0 0 4h4a2 2 0 1 1 0 4H8"/>
              <path d="M12 18V6"/>
            </svg>
          </div>
          <div class="card-content">
            <h3 class="card-title">รับสมัครเอเจนต์อสังหาฯ</h3>
            <p class="card-description">
              โอกาสสร้างรายได้ไม่จำกัด ค่าคอมมิชชั่นสูงสุด 50% พร้อมระบบสนับสนุนครบครัน
            </p>
            <div class="benefits">
              <div class="benefit-item">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <polyline points="20 6 9 17 4 12"/>
                </svg>
                ค่าคอมฯ สูงสุด 50%
              </div>
              <div class="benefit-item">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <polyline points="20 6 9 17 4 12"/>
                </svg>
                ระบบ CRM ฟรี
              </div>
              <div class="benefit-item">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <polyline points="20 6 9 17 4 12"/>
                </svg>
                อบรมฟรี
              </div>
            </div>
          </div>
          <NuxtLink to="/agent-registration" class="card-btn primary">
            สมัครเป็นเอเจนต์
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M5 12h14M12 5l7 7-7 7"/>
            </svg>
          </NuxtLink>
        </div>

        <!-- Partner & Investor -->
        <div class="announcement-card investor">
          <div class="card-icon">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M12 2v20M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/>
            </svg>
          </div>
          <div class="card-content">
            <h3 class="card-title">มองหา Partner & นักลงทุน</h3>
            <p class="card-description">
              เปิดโอกาสสำหรับ Developer, นักลงทุน และ Partner ที่ต้องการร่วมธุรกิจกับเรา
            </p>
            <div class="partner-types">
              <span class="partner-tag">Developer Partner</span>
              <span class="partner-tag">Investor</span>
              <span class="partner-tag">Strategic Partner</span>
            </div>
          </div>
          <NuxtLink to="/partnership" class="card-btn">
            ติดต่อเป็น Partner
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M5 12h14M12 5l7 7-7 7"/>
            </svg>
          </NuxtLink>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
</script>

<style scoped>
.announcements-section {
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
}

.section-title {
  font-size: 2.5rem;
  font-weight: 700;
  color: #1a1a1a;
  margin: 0;
}

.announcements-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 30px;
}

.announcement-card {
  position: relative;
  background: #fff;
  border-radius: 20px;
  padding: 35px;
  box-shadow: 0 5px 30px rgba(0, 0, 0, 0.06);
  display: flex;
  flex-direction: column;
  transition: all 0.3s ease;
}

.announcement-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 15px 50px rgba(212, 175, 55, 0.15);
}

.card-badge {
  position: absolute;
  top: 20px;
  right: 20px;
  background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%);
  color: #fff;
  padding: 6px 14px;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: 600;
}

.card-icon {
  width: 70px;
  height: 70px;
  border-radius: 50%;
  background: linear-gradient(135deg, rgba(212, 175, 55, 0.1) 0%, rgba(212, 175, 55, 0.05) 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 25px;
}

.card-icon svg {
  width: 32px;
  height: 32px;
  color: #DAA520;
}

.card-content {
  flex: 1;
}

.card-title {
  font-size: 1.4rem;
  font-weight: 700;
  color: #1a1a1a;
  margin: 0 0 12px 0;
}

.card-description {
  font-size: 0.95rem;
  color: #666;
  line-height: 1.7;
  margin: 0 0 20px 0;
}

.position-list {
  list-style: none;
  margin: 0;
  padding: 0;
}

.position-list li {
  padding: 8px 0;
  border-bottom: 1px dashed #eee;
  color: #666;
  font-size: 0.9rem;
}

.position-list li:last-child {
  border-bottom: none;
}

.benefits {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.benefit-item {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 0.9rem;
  color: #666;
}

.benefit-item svg {
  width: 18px;
  height: 18px;
  color: #22c55e;
}

.partner-types {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.partner-tag {
  background: rgba(212, 175, 55, 0.1);
  color: #b8860b;
  padding: 6px 14px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 500;
}

.card-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  background: transparent;
  border: 2px solid #1a1a1a;
  color: #1a1a1a;
  padding: 14px 24px;
  border-radius: 50px;
  font-size: 0.95rem;
  font-weight: 600;
  text-decoration: none;
  margin-top: 25px;
  transition: all 0.3s ease;
}

.card-btn svg {
  width: 18px;
  height: 18px;
  transition: transform 0.3s ease;
}

.card-btn:hover {
  background: #1a1a1a;
  color: #fff;
}

.card-btn:hover svg {
  transform: translateX(5px);
}

.card-btn.primary {
  background: linear-gradient(135deg, #DAA520 0%, #b8860b 100%);
  border-color: #DAA520;
  color: #fff;
}

.card-btn.primary:hover {
  box-shadow: 0 8px 25px rgba(212, 175, 55, 0.4);
}

/* Responsive */
@media (max-width: 991px) {
  .announcements-section {
    padding: 60px 0;
  }
  
  .section-title {
    font-size: 2rem;
  }
  
  .announcements-grid {
    grid-template-columns: 1fr;
    gap: 25px;
  }
  
  .announcement-card {
    max-width: 500px;
    margin: 0 auto;
  }
}

@media (max-width: 575px) {
  .section-title {
    font-size: 1.6rem;
  }
  
  .announcement-card {
    padding: 25px;
  }
  
  .card-title {
    font-size: 1.2rem;
  }
  
  .card-btn {
    width: 100%;
  }
}
</style>

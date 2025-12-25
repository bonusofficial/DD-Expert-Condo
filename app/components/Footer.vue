<template>
  <footer class="footer">
    <!-- Main Footer -->
    <div class="footer-main">
      <div class="footer-container">
        <!-- About Section -->
        <div class="footer-section footer-about">
          <div class="footer-logo">
            <img
              src="/logo.png"
              alt="DD Expert Condo"
              class="footer-logo-img"
            />
          </div>
          <p class="footer-description">
            DD Expert Condo ผู้เชี่ยวชาญด้านอสังหาริมทรัพย์ครบวงจร
            ให้บริการซื้อ-ขาย-เช่า คอนโดมิเนียม บ้านเดี่ยว ทาวน์เฮ้าส์ และที่ดิน
            พร้อมบริการที่ปรึกษาด้านสินเชื่อ และบริการช่างครบวงจร
            ด้วยทีมงานมืออาชีพที่พร้อมดูแลคุณอย่างใกล้ชิด
          </p>
          <div class="footer-social">
            <a
              href="https://facebook.com/ddexpert"
              target="_blank"
              class="social-link facebook"
              aria-label="Facebook"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="currentColor"
              >
                <path
                  d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"
                />
              </svg>
            </a>
            <a
              href="https://line.me/ti/p/@ddexpert"
              target="_blank"
              class="social-link line"
              aria-label="Line"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="currentColor"
              >
                <path
                  d="M19.365 9.863c.349 0 .63.285.63.631 0 .345-.281.63-.63.63H17.61v1.125h1.755c.349 0 .63.283.63.63 0 .344-.281.629-.63.629h-2.386c-.345 0-.627-.285-.627-.629V8.108c0-.345.282-.63.63-.63h2.386c.349 0 .63.285.63.63 0 .349-.281.63-.63.63H17.61v1.125h1.755zm-3.855 3.016c0 .27-.174.51-.432.596-.064.021-.133.031-.199.031-.211 0-.391-.09-.51-.25l-2.443-3.317v2.94c0 .344-.279.629-.631.629-.346 0-.626-.285-.626-.629V8.108c0-.27.173-.51.43-.595.06-.023.136-.033.194-.033.195 0 .375.104.495.254l2.462 3.33V8.108c0-.345.282-.63.63-.63.345 0 .63.285.63.63v4.771zm-5.741 0c0 .344-.282.629-.631.629-.345 0-.627-.285-.627-.629V8.108c0-.345.282-.63.63-.63.346 0 .628.285.628.63v4.771zm-2.466.629H4.917c-.345 0-.63-.285-.63-.629V8.108c0-.345.285-.63.63-.63.348 0 .63.285.63.63v4.141h1.756c.348 0 .629.283.629.63 0 .344-.282.629-.629.629M24 10.314C24 4.943 18.615.572 12 .572S0 4.943 0 10.314c0 4.811 4.27 8.842 10.035 9.608.391.082.923.258 1.058.59.12.301.079.766.038 1.08l-.164 1.02c-.045.301-.24 1.186 1.049.645 1.291-.539 6.916-4.078 9.436-6.975C23.176 14.393 24 12.458 24 10.314"
                />
              </svg>
            </a>
            <a
              href="https://tiktok.com/@ddexpert"
              target="_blank"
              class="social-link tiktok"
              aria-label="TikTok"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="currentColor"
              >
                <path
                  d="M19.59 6.69a4.83 4.83 0 0 1-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 0 1-5.2 1.74 2.89 2.89 0 0 1 2.31-4.64 2.93 2.93 0 0 1 .88.13V9.4a6.84 6.84 0 0 0-1-.05A6.33 6.33 0 0 0 5 20.1a6.34 6.34 0 0 0 10.86-4.43v-7a8.16 8.16 0 0 0 4.77 1.52v-3.4a4.85 4.85 0 0 1-1-.1z"
                />
              </svg>
            </a>
            <a
              href="https://youtube.com/@ddexpert"
              target="_blank"
              class="social-link youtube"
              aria-label="YouTube"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="currentColor"
              >
                <path
                  d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"
                />
              </svg>
            </a>
          </div>
        </div>

        <!-- Quick Links -->
        <div class="footer-section">
          <h3 class="footer-title">บริการของเรา</h3>
          <ul class="footer-links">
            <li><NuxtLink to="/buy">ซื้ออสังหาริมทรัพย์</NuxtLink></li>
            <li><NuxtLink to="/sell">ขายอสังหาริมทรัพย์</NuxtLink></li>
            <li><NuxtLink to="/rent">เช่าอสังหาริมทรัพย์</NuxtLink></li>
            <li><NuxtLink to="/mortgage">จำนอง</NuxtLink></li>
            <li><NuxtLink to="/sale-redemption">ขายฝาก</NuxtLink></li>
            <li>
              <NuxtLink to="/loan-consultant">ที่ปรึกษาด้านสินเชื่อ</NuxtLink>
            </li>
            <li><NuxtLink to="/services">บริการช่างครบวงจร</NuxtLink></li>
          </ul>
        </div>

        <!-- Property Types -->
        <div class="footer-section">
          <h3 class="footer-title">ประเภทอสังหาฯ</h3>
          <ul class="footer-links">
            <li><NuxtLink to="/condo">คอนโดมิเนียม</NuxtLink></li>
            <li><NuxtLink to="/house">บ้านเดี่ยว</NuxtLink></li>
            <li><NuxtLink to="/townhouse">ทาวน์เฮ้าส์</NuxtLink></li>
            <li><NuxtLink to="/land">ที่ดิน</NuxtLink></li>
            <li><NuxtLink to="/commercial">อาคารพาณิชย์</NuxtLink></li>
          </ul>
        </div>

        <!-- Contact Info -->
        <div class="footer-section footer-contact">
          <h3 class="footer-title">ติดต่อเรา</h3>
          <ul class="contact-list">
            <li class="contact-item">
              <div class="contact-icon">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z" />
                  <circle cx="12" cy="10" r="3" />
                </svg>
              </div>
              <div class="contact-info">
                <span class="contact-label">ที่อยู่</span>
                <span class="contact-value"
                  >123 อาคาร DD Expert ถ.สุขุมวิท แขวงคลองเตย เขตคลองเตย
                  กรุงเทพฯ 10110</span
                >
              </div>
            </li>
            <li class="contact-item">
              <div class="contact-icon">
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
              </div>
              <div class="contact-info">
                <span class="contact-label">โทรศัพท์</span>
                <a href="tel:02-XXX-XXXX" class="contact-value contact-link"
                  >02-XXX-XXXX</a
                >
              </div>
            </li>
            <li class="contact-item">
              <div class="contact-icon">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <path
                    d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"
                  />
                  <polyline points="22,6 12,13 2,6" />
                </svg>
              </div>
              <div class="contact-info">
                <span class="contact-label">อีเมล</span>
                <a
                  href="mailto:info@ddexpert.co.th"
                  class="contact-value contact-link"
                  >info@ddexpert.co.th</a
                >
              </div>
            </li>
            <li class="contact-item">
              <div class="contact-icon line-icon">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="currentColor"
                >
                  <path
                    d="M19.365 9.863c.349 0 .63.285.63.631 0 .345-.281.63-.63.63H17.61v1.125h1.755c.349 0 .63.283.63.63 0 .344-.281.629-.63.629h-2.386c-.345 0-.627-.285-.627-.629V8.108c0-.345.282-.63.63-.63h2.386c.349 0 .63.285.63.63 0 .349-.281.63-.63.63H17.61v1.125h1.755zm-3.855 3.016c0 .27-.174.51-.432.596-.064.021-.133.031-.199.031-.211 0-.391-.09-.51-.25l-2.443-3.317v2.94c0 .344-.279.629-.631.629-.346 0-.626-.285-.626-.629V8.108c0-.27.173-.51.43-.595.06-.023.136-.033.194-.033.195 0 .375.104.495.254l2.462 3.33V8.108c0-.345.282-.63.63-.63.345 0 .63.285.63.63v4.771zm-5.741 0c0 .344-.282.629-.631.629-.345 0-.627-.285-.627-.629V8.108c0-.345.282-.63.63-.63.346 0 .628.285.628.63v4.771zm-2.466.629H4.917c-.345 0-.63-.285-.63-.629V8.108c0-.345.285-.63.63-.63.348 0 .63.285.63.63v4.141h1.756c.348 0 .629.283.629.63 0 .344-.282.629-.629.629M24 10.314C24 4.943 18.615.572 12 .572S0 4.943 0 10.314c0 4.811 4.27 8.842 10.035 9.608.391.082.923.258 1.058.59.12.301.079.766.038 1.08l-.164 1.02c-.045.301-.24 1.186 1.049.645 1.291-.539 6.916-4.078 9.436-6.975C23.176 14.393 24 12.458 24 10.314"
                  />
                </svg>
              </div>
              <div class="contact-info">
                <span class="contact-label">Line Official</span>
                <a
                  href="https://line.me/ti/p/@ddexpert"
                  target="_blank"
                  class="contact-value contact-link"
                  >@ddexpert</a
                >
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <!-- Footer Bottom -->
    <div class="footer-bottom">
      <div class="footer-container">
        <p class="copyright">
          © {{ currentYear }} DD Expert Condo. All Rights Reserved.
        </p>
        <div class="footer-bottom-links">
          <NuxtLink to="/privacy-policy">นโยบายความเป็นส่วนตัว</NuxtLink>
          <span class="divider">|</span>
          <NuxtLink to="/terms">ข้อกำหนดการใช้งาน</NuxtLink>
        </div>
      </div>
    </div>
  </footer>
</template>

<script setup>
const currentYear = new Date().getFullYear();
</script>

<style scoped>
.footer {
  background: linear-gradient(180deg, #1a1a1a 0%, #0d0d0d 100%);
  color: #fff;
}

.footer-main {
  padding: 60px 0 40px;
  border-bottom: 1px solid rgba(212, 175, 55, 0.2);
}

.footer-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 20px;
  display: grid;
  grid-template-columns: 1.5fr 1fr 1fr 1.5fr;
  gap: 40px;
}

/* About Section */
.footer-about {
  padding-right: 30px;
}

.footer-logo-img {
  height: 50px;
  width: auto;
  margin-bottom: 20px;
}

.footer-description {
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9rem;
  line-height: 1.8;
  margin-bottom: 25px;
}

.footer-social {
  display: flex;
  gap: 12px;
}

.social-link {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: #fff;
}

.social-link svg {
  width: 18px;
  height: 18px;
}

.social-link:hover {
  transform: translateY(-3px);
  border-color: transparent;
}

.social-link.facebook:hover {
  background: #1877f2;
  box-shadow: 0 5px 15px rgba(24, 119, 242, 0.4);
}

.social-link.line:hover {
  background: #00c300;
  box-shadow: 0 5px 15px rgba(0, 195, 0, 0.4);
}

.social-link.tiktok:hover {
  background: #000;
  border-color: #fff;
  box-shadow: 0 5px 15px rgba(255, 255, 255, 0.2);
}

.social-link.youtube:hover {
  background: #ff0000;
  box-shadow: 0 5px 15px rgba(255, 0, 0, 0.4);
}

/* Footer Sections */
.footer-title {
  font-size: 1.1rem;
  font-weight: 600;
  color: #DAA520;
  margin-bottom: 25px;
  position: relative;
  padding-bottom: 12px;
}

.footer-title::after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  width: 40px;
  height: 2px;
  background: linear-gradient(90deg, #DAA520, #f4e5b2);
}

.footer-links {
  list-style: none;
  margin: 0;
  padding: 0;
}

.footer-links li {
  margin-bottom: 12px;
}

.footer-links a {
  color: rgba(255, 255, 255, 0.7);
  text-decoration: none;
  font-size: 0.9rem;
  transition: all 0.3s ease;
  display: inline-block;
}

.footer-links a:hover {
  color: #DAA520;
  transform: translateX(5px);
}

/* Contact Section */
.contact-list {
  list-style: none;
  margin: 0;
  padding: 0;
}

.contact-item {
  display: flex;
  gap: 15px;
  margin-bottom: 20px;
}

.contact-icon {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: rgba(212, 175, 55, 0.1);
  border: 1px solid rgba(212, 175, 55, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.contact-icon svg {
  width: 18px;
  height: 18px;
  color: #DAA520;
}

.contact-icon.line-icon svg {
  width: 20px;
  height: 20px;
}

.contact-info {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.contact-label {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.5);
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.contact-value {
  color: rgba(255, 255, 255, 0.9);
  font-size: 0.9rem;
  line-height: 1.5;
}

.contact-link {
  text-decoration: none;
  transition: color 0.3s ease;
}

.contact-link:hover {
  color: #DAA520;
}

/* Footer Bottom */
.footer-bottom {
  padding: 20px 0;
}

.footer-bottom .footer-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.copyright {
  color: rgba(255, 255, 255, 0.5);
  font-size: 0.85rem;
  margin: 0;
}

.footer-bottom-links {
  display: flex;
  align-items: center;
  gap: 15px;
}

.footer-bottom-links a {
  color: rgba(255, 255, 255, 0.5);
  text-decoration: none;
  font-size: 0.85rem;
  transition: color 0.3s ease;
}

.footer-bottom-links a:hover {
  color: #DAA520;
}

.divider {
  color: rgba(255, 255, 255, 0.3);
}

/* Responsive */
@media (max-width: 1199px) {
  .footer-container {
    grid-template-columns: repeat(2, 1fr);
  }

  .footer-about {
    grid-column: span 2;
    padding-right: 0;
  }
}

@media (max-width: 767px) {
  .footer-main {
    padding: 40px 0 30px;
  }

  .footer-container {
    grid-template-columns: 1fr;
    gap: 30px;
  }

  .footer-about {
    grid-column: span 1;
    text-align: center;
  }

  .footer-social {
    justify-content: center;
  }

  .footer-title {
    text-align: center;
  }

  .footer-title::after {
    left: 50%;
    transform: translateX(-50%);
  }

  .footer-links {
    text-align: center;
  }

  .footer-links a:hover {
    transform: none;
  }

  .contact-item {
    justify-content: center;
    text-align: left;
  }

  .footer-bottom .footer-container {
    flex-direction: column;
    gap: 15px;
    text-align: center;
  }
}

@media (max-width: 575px) {
  .footer-main {
    padding: 30px 0 20px;
  }

  .footer-logo-img {
    height: 40px;
  }

  .footer-description {
    font-size: 0.85rem;
  }

  .footer-title {
    font-size: 1rem;
    margin-bottom: 20px;
  }

  .social-link {
    width: 36px;
    height: 36px;
  }

  .social-link svg {
    width: 16px;
    height: 16px;
  }

  .contact-icon {
    width: 36px;
    height: 36px;
  }

  .contact-icon svg {
    width: 16px;
    height: 16px;
  }

  .contact-value {
    font-size: 0.85rem;
  }

  .copyright,
  .footer-bottom-links a {
    font-size: 0.8rem;
  }
}
</style>

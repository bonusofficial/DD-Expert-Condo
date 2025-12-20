<template>
  <div class="side-popup">
    <!-- Toggle Button -->
    <button class="popup-toggle" :class="{ open: isOpen }" @click="togglePopup">
      <svg
        v-if="!isOpen"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
      >
        <path
          d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"
        />
      </svg>
      <svg
        v-else
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

    <!-- Social Buttons -->
    <transition-group name="social-btn" tag="div" class="social-buttons">
      <a
        v-for="(item, index) in socialItems"
        v-show="isOpen"
        :key="item.name"
        :href="item.link"
        :target="item.external ? '_blank' : '_self'"
        :class="['social-btn', item.class]"
        :style="{ transitionDelay: `${index * 0.05}s` }"
        @mouseenter="activeTooltip = item.name"
        @mouseleave="activeTooltip = null"
      >
        <span v-html="item.icon"></span>

        <!-- Tooltip -->
        <transition name="tooltip">
          <div v-if="activeTooltip === item.name" class="tooltip">
            {{ item.tooltip }}
          </div>
        </transition>
      </a>
    </transition-group>
  </div>
</template>

<script setup>
import { ref } from "vue";

const isOpen = ref(false);
const activeTooltip = ref(null);

const togglePopup = () => {
  isOpen.value = !isOpen.value;
  if (!isOpen.value) {
    activeTooltip.value = null;
  }
};

const socialItems = [
  {
    name: "call",
    tooltip: "โทร 02-XXX-XXXX",
    link: "tel:02-XXX-XXXX",
    external: false,
    class: "btn-call",
    icon: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/>
    </svg>`,
  },
  {
    name: "facebook",
    tooltip: "Facebook DDexpert",
    link: "https://facebook.com/ddexpert",
    external: true,
    class: "btn-facebook",
    icon: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor">
      <path d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"/>
    </svg>`,
  },
  {
    name: "line",
    tooltip: "Line @ddexpert",
    link: "https://line.me/ti/p/@ddexpert",
    external: true,
    class: "btn-line",
    icon: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor">
      <path d="M19.365 9.863c.349 0 .63.285.63.631 0 .345-.281.63-.63.63H17.61v1.125h1.755c.349 0 .63.283.63.63 0 .344-.281.629-.63.629h-2.386c-.345 0-.627-.285-.627-.629V8.108c0-.345.282-.63.63-.63h2.386c.349 0 .63.285.63.63 0 .349-.281.63-.63.63H17.61v1.125h1.755zm-3.855 3.016c0 .27-.174.51-.432.596-.064.021-.133.031-.199.031-.211 0-.391-.09-.51-.25l-2.443-3.317v2.94c0 .344-.279.629-.631.629-.346 0-.626-.285-.626-.629V8.108c0-.27.173-.51.43-.595.06-.023.136-.033.194-.033.195 0 .375.104.495.254l2.462 3.33V8.108c0-.345.282-.63.63-.63.345 0 .63.285.63.63v4.771zm-5.741 0c0 .344-.282.629-.631.629-.345 0-.627-.285-.627-.629V8.108c0-.345.282-.63.63-.63.346 0 .628.285.628.63v4.771zm-2.466.629H4.917c-.345 0-.63-.285-.63-.629V8.108c0-.345.285-.63.63-.63.348 0 .63.285.63.63v4.141h1.756c.348 0 .629.283.629.63 0 .344-.282.629-.629.629M24 10.314C24 4.943 18.615.572 12 .572S0 4.943 0 10.314c0 4.811 4.27 8.842 10.035 9.608.391.082.923.258 1.058.59.12.301.079.766.038 1.08l-.164 1.02c-.045.301-.24 1.186 1.049.645 1.291-.539 6.916-4.078 9.436-6.975C23.176 14.393 24 12.458 24 10.314"/>
    </svg>`,
  },
  {
    name: "tiktok",
    tooltip: "TikTok @ddexpert",
    link: "https://tiktok.com/@ddexpert",
    external: true,
    class: "btn-tiktok",
    icon: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor">
      <path d="M19.59 6.69a4.83 4.83 0 0 1-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 0 1-5.2 1.74 2.89 2.89 0 0 1 2.31-4.64 2.93 2.93 0 0 1 .88.13V9.4a6.84 6.84 0 0 0-1-.05A6.33 6.33 0 0 0 5 20.1a6.34 6.34 0 0 0 10.86-4.43v-7a8.16 8.16 0 0 0 4.77 1.52v-3.4a4.85 4.85 0 0 1-1-.1z"/>
    </svg>`,
  },
  {
    name: "youtube",
    tooltip: "YouTube DDexpert",
    link: "https://youtube.com/@ddexpert",
    external: true,
    class: "btn-youtube",
    icon: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor">
      <path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/>
    </svg>`,
  },
];
</script>

<style scoped>
.side-popup {
  position: fixed;
  bottom: 30px;
  right: 30px;
  z-index: 999;
  display: flex;
  flex-direction: column-reverse;
  align-items: center;
  gap: 12px;
}

/* Toggle Button */
.popup-toggle {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 6px 25px rgba(212, 175, 55, 0.4);
  transition: all 0.3s ease;
  z-index: 10;
}

.popup-toggle:hover {
  transform: scale(1.1);
  box-shadow: 0 8px 30px rgba(212, 175, 55, 0.5);
}

.popup-toggle.open {
  background: linear-gradient(135deg, #2c2c2c 0%, #1a1a1a 100%);
  box-shadow: 0 6px 25px rgba(0, 0, 0, 0.3);
}

.popup-toggle svg {
  width: 28px;
  height: 28px;
  color: #fff;
}

/* Social Buttons Container */
.social-buttons {
  display: flex;
  flex-direction: column-reverse;
  gap: 12px;
}

/* Social Button Base */
.social-btn {
  position: relative;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  text-decoration: none;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.social-btn:hover {
  transform: scale(1.15);
}

.social-btn :deep(svg) {
  width: 24px;
  height: 24px;
  fill: currentColor;
}

.social-btn :deep(svg path) {
  fill: currentColor;
}

.btn-call :deep(svg) {
  fill: none;
  stroke: currentColor;
  stroke-width: 2;
}

/* Call Button */
.btn-call {
  background: linear-gradient(135deg, #d4af37 0%, #b8860b 100%);
  color: #fff;
}

.btn-call:hover {
  box-shadow: 0 6px 20px rgba(212, 175, 55, 0.5);
}

/* Facebook Button */
.btn-facebook {
  background: linear-gradient(135deg, #1877f2 0%, #0d5cbd 100%);
  color: #fff;
}

.btn-facebook:hover {
  box-shadow: 0 6px 20px rgba(24, 119, 242, 0.5);
}

/* Line Button */
.btn-line {
  background: linear-gradient(135deg, #00c300 0%, #00a000 100%);
  color: #fff;
}

.btn-line:hover {
  box-shadow: 0 6px 20px rgba(0, 195, 0, 0.5);
}

/* TikTok Button */
.btn-tiktok {
  background: linear-gradient(135deg, #000000 0%, #333333 100%);
  color: #fff;
}

.btn-tiktok:hover {
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.5);
}

/* YouTube Button */
.btn-youtube {
  background: linear-gradient(135deg, #ff0000 0%, #cc0000 100%);
  color: #fff;
}

.btn-youtube:hover {
  box-shadow: 0 6px 20px rgba(255, 0, 0, 0.5);
}

/* Tooltip */
.tooltip {
  position: absolute;
  right: calc(100% + 15px);
  top: 50%;
  transform: translateY(-50%);
  background: linear-gradient(135deg, #2c2c2c 0%, #1a1a1a 100%);
  color: #fff;
  padding: 10px 16px;
  border-radius: 8px;
  font-size: 0.85rem;
  font-weight: 500;
  white-space: nowrap;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  pointer-events: none;
}

.tooltip::after {
  content: "";
  position: absolute;
  right: -8px;
  top: 50%;
  transform: translateY(-50%);
  border: 8px solid transparent;
  border-left-color: #2c2c2c;
  border-right: none;
}

/* Animations */
.social-btn-enter-active,
.social-btn-leave-active {
  transition: all 0.3s ease;
}

.social-btn-enter-from,
.social-btn-leave-to {
  opacity: 0;
  transform: scale(0.5) translateY(20px);
}

.tooltip-enter-active,
.tooltip-leave-active {
  transition: all 0.2s ease;
}

.tooltip-enter-from,
.tooltip-leave-to {
  opacity: 0;
  transform: translateY(-50%) translateX(10px);
}

/* Responsive */
@media (max-width: 767px) {
  .side-popup {
    bottom: 20px;
    right: 20px;
    gap: 10px;
  }

  .popup-toggle {
    width: 55px;
    height: 55px;
  }

  .popup-toggle svg {
    width: 24px;
    height: 24px;
  }

  .social-btn {
    width: 45px;
    height: 45px;
  }

  .social-btn svg {
    width: 20px;
    height: 20px;
  }

  .tooltip {
    display: none;
  }
}

@media (max-width: 575px) {
  .side-popup {
    bottom: 15px;
    right: 15px;
  }

  .popup-toggle {
    width: 50px;
    height: 50px;
  }

  .social-btn {
    width: 42px;
    height: 42px;
  }

  .social-buttons {
    gap: 8px;
  }
}
</style>

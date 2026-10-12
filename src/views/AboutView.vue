<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'
import { useI18n } from 'vue-i18n'
import about1 from '../assets/images/about1.png'
import about2 from '../assets/images/about2.png'
import AppNavBar from '../components/AppNavBar.vue'
import AboutFooter from '../components/AboutFooter.vue'

const { t } = useI18n()

/* ------------------------------------------------------------------
   比例：正文 : footer = 4 : 1
   正文高度是内容撑开的（不是固定值），纯 CSS 拿不到它的实际像素，
   所以这里用一小段 JS 把 footer 的 min-height 同步为正文高度的 1/4。
   用 ResizeObserver 监听，窗口缩放或内容变化时自动重算。
   ------------------------------------------------------------------ */
const mainRef = ref(null)
const footerSlotRef = ref(null)
let observer = null

function syncFooterHeight() {
  const mainEl = mainRef.value
  const footerEl = footerSlotRef.value
  if (!mainEl || !footerEl) return
  footerEl.style.minHeight = `${Math.round(mainEl.offsetHeight / 4)}px`
}

onMounted(async () => {
  await nextTick()
  syncFooterHeight()

  if (typeof ResizeObserver !== 'undefined') {
    observer = new ResizeObserver(syncFooterHeight)
    if (mainRef.value) observer.observe(mainRef.value)
  } else {
    window.addEventListener('resize', syncFooterHeight)
  }
})

onUnmounted(() => {
  observer?.disconnect()
  window.removeEventListener('resize', syncFooterHeight)
})
</script>

<template>
  <main class="about-page">
    <!-- ① 左侧黑色栏：宽 : 右侧 = 1 : 3，纵跨整个高度 -->
    <aside class="about-sidebar">
      <span class="sidebar-title">About</span>
    </aside>

    <!-- ② 右侧正文 + ③ footer：高 = 4 : 1 -->
    <div class="about-content">
      <AppNavBar />

      <section class="about-main" ref="mainRef">
        <div class="main-grid">
          <!-- 正文左栏 -->
          <div class="col-left">
            <h2 class="designer-title">Designer introduction</h2>

            <div class="designer-copy">
              <p>{{ t('about.storyP1') }}</p>
              <p>{{ t('about.storyP2') }}</p>
              <p>{{ t('about.storyP3') }}</p>
            </div>

            <div class="about-portrait">
              <img :src="about1" alt="Designer portrait" />
            </div>
          </div>

          <!-- 正文右栏 -->
          <div class="col-right">
            <span class="about-title">About</span>

            <h2 class="press-title">
              <span>14</span> Best Bridal Designers in Asia
            </h2>

            <div class="press-copy">
              <p>{{ t('about.storyP4') }}</p>
              <p>{{ t('about.storyP5') }}</p>
            </div>

            <div class="about-studio">
              <img :src="about2" alt="Studio" />
            </div>
          </div>
        </div>
      </section>

      <AboutFooter />
    </div>
  </main>
</template>

<style scoped>
/* ============================================================
   页面骨架：两列网格
   列宽比例 → 黑色左栏 : 右侧内容 = 1 : 3
   ============================================================ */
.about-page {
  display: grid;
  grid-template-columns: 1fr 3fr;
  min-height: 100vh;
  background: var(--cream);
}

/* ===== ① 黑色左栏（纵跨全高） ===== */
.about-sidebar {
  background: #000;
  padding: calc(52 * var(--unit)) calc(44 * var(--unit));
}

.sidebar-title {
  display: block;
  font-family: Georgia, 'Times New Roman', 'Noto Serif SC', serif;
  font-size: clamp(30px, 3.1vw, 60px);
  font-weight: 700;
  color: #8f8f8f;
  line-height: 1;
  letter-spacing: 0.01em;
}

/* ============================================================
   ② 右侧：正文 + ③ footer
   高度比例 → 正文 : footer = 4 : 1
   用 flex-grow 4 / 1 实现；内容超出时各自撑开，页面可滚动
   ============================================================ */
.about-content {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  min-width: 0;
}

.about-main {
  flex: 4 0 auto;
}

/* footer 槽位：min-height 由 JS 同步为正文高度的 1/4（正文 : footer = 4 : 1） */
.about-footer-slot {
  flex: 1 0 auto;
  display: flex;
  flex-direction: column;
}

.about-footer-slot > * {
  flex: 1 0 auto;
}

/* ===== 导航栏在窄列内的位置修正 ===== */
.about-content :deep(.app-navbar) {
  flex: none;
  margin-top: calc(38 * var(--unit));
  margin-bottom: calc(44 * var(--unit));
}

.about-content :deep(.navbar-nav-left) {
  left: calc(64 * var(--unit));
}

.about-content :deep(.navbar-nav-right) {
  right: calc(64 * var(--unit));
}

.about-content :deep(.logo) {
  width: calc(96 * var(--unit));
}

/* ===== 正文内部：左右两栏 ===== */
.main-grid {
  display: grid;
  grid-template-columns: 1fr 0.95fr;
  column-gap: 9%;
  align-items: start;
  padding: 0 calc(64 * var(--unit)) calc(64 * var(--unit));
}

.col-left,
.col-right {
  min-width: 0;
}

/* ===== 左栏：Designer introduction ===== */
.designer-title {
  font-family: var(--font-script);
  font-size: calc(30 * var(--unit));
  font-weight: 400;
  color: var(--blue);
  line-height: 1.1;
  letter-spacing: calc(1.4 * var(--unit));
  margin: 0 0 calc(14 * var(--unit));
  white-space: nowrap;
}

.designer-copy p {
  font-family: var(--font-sans);
  font-size: calc(15 * var(--unit));
  line-height: calc(24 * var(--unit));
  letter-spacing: calc(0.8 * var(--unit));
  color: var(--color-text);
  margin: 0 0 calc(18 * var(--unit));
}

.designer-copy p:last-child {
  margin-bottom: 0;
}

.about-portrait {
  margin-top: calc(36 * var(--unit));
}

.about-portrait img {
  width: 100%;
  height: auto;
  display: block;
}

/* ===== 右栏：About + Press ===== */
.about-title {
  display: block;
  font-family: var(--font-script);
  font-size: calc(84 * var(--unit));
  font-weight: 400;
  color: var(--color-text);
  line-height: 0.8;
  margin: 0 0 calc(150 * var(--unit));
}

.press-title {
  font-family: var(--font-script);
  font-size: calc(30 * var(--unit));
  font-weight: 400;
  color: var(--blue);
  line-height: 1.1;
  letter-spacing: calc(1.4 * var(--unit));
  margin: 0 0 calc(14 * var(--unit));
}

.press-title span {
  font-size: calc(44 * var(--unit));
  letter-spacing: 0;
}

.press-copy p {
  font-family: var(--font-sans);
  font-size: calc(15 * var(--unit));
  line-height: calc(24 * var(--unit));
  letter-spacing: calc(0.8 * var(--unit));
  color: var(--color-text-light);
  margin: 0 0 calc(18 * var(--unit));
}

.press-copy p:last-child {
  margin-bottom: 0;
}

.about-studio {
  margin-top: calc(36 * var(--unit));
}

.about-studio img {
  width: 100%;
  height: auto;
  display: block;
}

/* ============================================================
   响应式：窄屏改为单列，黑栏变成顶部色块
   ============================================================ */
@media (max-width: 1024px) {
  .about-page {
    grid-template-columns: 1fr;
  }

  .about-sidebar {
    padding: 36px 32px;
  }

  .sidebar-title {
    font-size: 2.4rem;
  }

  .main-grid {
    grid-template-columns: 1fr;
    row-gap: 48px;
    padding: 0 32px 48px;
  }

  .about-content :deep(.navbar-nav-left) {
    left: 32px;
  }

  .about-content :deep(.navbar-nav-right) {
    right: 32px;
  }

  .designer-title,
  .press-title {
    font-size: 1.5rem;
    white-space: normal;
  }

  .designer-copy p,
  .press-copy p {
    font-size: 0.9rem;
    line-height: 1.65;
  }

  .about-title {
    font-size: 3.5rem;
    margin-bottom: 48px;
  }
}

@media (max-width: 767px) {
  .about-sidebar {
    padding: 28px 24px;
  }

  .sidebar-title {
    font-size: 2rem;
  }

  .about-content :deep(.app-navbar) {
    margin-top: 20px;
    margin-bottom: 24px;
  }

  .main-grid {
    padding: 0 24px 36px;
    row-gap: 36px;
  }

  .about-title {
    font-size: 2.75rem;
    margin-bottom: 32px;
  }

  .press-title span {
    font-size: 1.9rem;
  }
}
</style>

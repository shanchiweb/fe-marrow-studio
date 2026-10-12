<script setup>
import { useI18n } from 'vue-i18n'

const { t } = useI18n()
</script>

<template>
  <footer class="about-footer">
    <div class="footer-inner">
      <!-- 第一栏：系列 -->
      <nav class="footer-col footer-col--series">
        <a class="footer-link footer-link--accent" href="#">{{ t('about.series1') }}</a>
        <a class="footer-link footer-link--accent" href="#">{{ t('about.series2') }}</a>
        <a class="footer-link footer-link--accent" href="#">{{ t('about.series3') }}</a>
      </nav>

      <!-- 第二栏：页面链接 -->
      <nav class="footer-col">
        <a class="footer-link" href="#">{{ t('about.footerAbout') }}</a>
        <a class="footer-link" href="#">{{ t('about.footerBoutique') }}</a>
        <a class="footer-link footer-link--muted" href="#">{{ t('about.footerStockists') }}</a>
      </nav>

      <!-- 第三栏：联系方式 -->
      <div class="footer-col">
        <a class="footer-link footer-link--contact" href="#">
          Rednote<span class="link-mark" aria-hidden="true">↗</span>
        </a>
        <a class="footer-link footer-link--contact" href="#">
          Instagram<span class="link-mark" aria-hidden="true">↗</span>
        </a>
        <a class="footer-link footer-link--contact" :href="`mailto:${t('about.email')}`">
          Email<span class="link-mark" aria-hidden="true">↗</span>
        </a>
        <a class="footer-link footer-link--contact" :href="`tel:${t('about.tel')}`">
          Tel<span class="link-mark" aria-hidden="true">↗</span>
        </a>
        <a class="footer-link footer-link--contact" href="#">
          Address<span class="link-mark" aria-hidden="true">↗</span>
        </a>
      </div>

      <!-- 第四栏：邮箱订阅 -->
      <div class="footer-col footer-col--subscribe">
        <input
          class="subscribe-input"
          type="email"
          :placeholder="t('about.subscribePlaceholder')"
          autocomplete="email"
        />
        <p class="subscribe-note">{{ t('about.subscribeNote') }}</p>
      </div>
    </div>
  </footer>
</template>

<style scoped>
/* ===== 页脚容器 ===== */
.about-footer {
  border-top: 1px solid var(--color-primary);
  background: transparent;
}

.footer-inner {
  display: grid;
  grid-template-columns: 1fr 1.15fr 1.15fr 1.7fr;
  gap: calc(32 * var(--unit));
  padding: calc(46 * var(--unit)) calc(64 * var(--unit)) calc(54 * var(--unit));
}

/* ===== 通用列 ===== */
.footer-col {
  display: flex;
  flex-direction: column;
  gap: calc(9 * var(--unit));
  min-width: 0;
}

/* ===== 链接 ===== */
.footer-link {
  font-family: var(--font-sans);
  font-size: calc(15 * var(--unit));
  line-height: 1.5;
  letter-spacing: calc(0.6 * var(--unit));
  color: var(--color-text-light);
  text-decoration: none;
  transition: color var(--transition-fast);
  white-space: nowrap;
  width: fit-content;
}

.footer-link:hover {
  color: var(--color-primary-dark);
}

.footer-link--accent {
  color: var(--color-primary);
}

.footer-link--muted {
  color: var(--color-text-muted);
}

.footer-link--contact {
  display: inline-flex;
  align-items: center;
  gap: calc(6 * var(--unit));
}

.link-mark {
  font-size: 0.85em;
  line-height: 1;
  opacity: 0.75;
}

/* ===== 邮箱订阅 ===== */
.footer-col--subscribe {
  gap: calc(10 * var(--unit));
  justify-content: flex-start;
}

.subscribe-input {
  width: 100%;
  max-width: calc(430 * var(--unit));
  height: calc(40 * var(--unit));
  padding: 0 calc(12 * var(--unit));
  border: 1px solid var(--color-primary);
  border-radius: 2px;
  background: transparent;
  font-family: var(--font-sans);
  font-size: calc(14 * var(--unit));
  color: var(--color-text);
  outline: none;
  transition: border-color var(--transition-fast);
}

.subscribe-input::placeholder {
  color: var(--color-text-muted);
}

.subscribe-input:focus {
  border-color: var(--color-primary-dark);
}

.subscribe-note {
  font-family: var(--font-sans);
  font-size: calc(13 * var(--unit));
  line-height: 1.5;
  letter-spacing: calc(0.4 * var(--unit));
  color: var(--color-text-muted);
  max-width: calc(430 * var(--unit));
}

/* ===== 响应式 ===== */
@media (max-width: 1024px) {
  .footer-inner {
    grid-template-columns: 1fr 1fr;
    gap: 28px;
    padding: 36px 32px 44px;
  }

  .footer-link {
    font-size: 0.9rem;
    white-space: normal;
  }

  .subscribe-input {
    max-width: 100%;
    height: 40px;
    font-size: 0.9rem;
  }

  .subscribe-note {
    font-size: 0.8rem;
    max-width: 100%;
  }
}

@media (max-width: 767px) {
  .footer-inner {
    grid-template-columns: 1fr;
    gap: 24px;
    padding: 28px 24px 36px;
  }
}
</style>

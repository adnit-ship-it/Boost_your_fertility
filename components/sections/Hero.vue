<template>
  <div v-if="hero?.show !== false" class="relative w-full mx-auto h-[85vh] md:h-[960px] text-black">
    <!-- Background Image -->
    <div class="absolute inset-0 bg-cover bg-no-repeat w-full h-full" :style="{
        backgroundImage: `url(${hero?.media?.background?.src || '/assets/images/brand/hero-bg.png'})`,
        backgroundPosition: 'bottom right',
      }"></div>

    <!-- Content - Single Column -->
    <div class="w-full max-w-[1280px] px-0 h-full md:px-8 mx-auto">
      <div class="relative z-10 max-w-3xl h-full">
        <div class="flex pt-16 lg:pt-0 flex-col justify-center h-full bg-[#ebf2ee]/70 md:bg-white/0 px-4">
          <img 
            :src="hero?.media?.logo?.src || '/assets/images/brand/logo.svg'" 
            :alt="hero?.media?.logo?.alt || 'Brand Logo'"
            class="w-[200px] md:w-[365px] object-cover -ml-2 logo-hero"
            :style="{ height: getLogoHeight() }"
          />
          <h1 class="text-3xl lg:text-[48px] font-semibold text-bodyColor mt-0 lg:mt-[34px]">
            {{ hero?.heading || 'Confidence, We Got You' }}
          </h1>

          <h2 class="text-xl lg:text-[34px] text-bodyColor mt-[0px] lg:mt-[24px]">
            {{ hero?.subheading || 'Personalized GLP-1 Medication' }}
          </h2>
          <!-- Bullet Points -->
          <ul class="space-y-4 text-[16px] md:text-[24px] mt-3 lg:mt-[28px]">
            <li 
              v-for="(bulletPoint, index) in hero?.bulletPoints" 
              :key="index"
              class="flex items-center text-accentColor1"
            >
              <UiIcon 
                :icon-type="bulletPoint.iconType || 'checkmark'"
                :icon-color="bulletPoint.iconColor || '#A75809'"
                :size="isMobile ? '20px' : '26px'"
                :alt="`${bulletPoint.iconType} icon`"
                class="mr-3"
              />
              <span>{{ bulletPoint.text }}</span>
            </li>
          </ul>

          <!-- CTA Button -->
          <NuxtLink class="mt-6 lg:mt-12" to="/consultation">
            <button class="h-[40px] lg:h-[44px] w-[224px] lg:w-[320px] text-[20px] lg:text-[24px] flex items-center justify-center font-medium transition-colors rounded-full px-2 md:px-6 bg-accentColor1 text-white cursor-pointer">
              {{ hero?.ctaButton || 'Take the Assessment' }}
            </button>
          </NuxtLink>
        </div>
      </div>
    </div>

  </div>
</template>

<script setup>
import { computed, ref, onMounted, onUnmounted } from 'vue'
import { useSiteTextStore } from '~/stores/siteText'

const siteTextStore = useSiteTextStore()
const hero = computed(() => siteTextStore.getHomeText()?.hero)
const isMobile = ref(true)
const isTablet = ref(false)

const checkScreenSize = () => {
  if (typeof window !== 'undefined') {
    isMobile.value = window.innerWidth < 768
    isTablet.value = window.innerWidth >= 768 && window.innerWidth < 1024
  }
}

const getLogoHeight = () => {
  if (!hero.value?.media?.logo?.heights) return '24px'
  if (isMobile.value) return hero.value.media.logo.heights.mobile || '24px'
  if (isTablet.value) return hero.value.media.logo.heights.tablet || '28px'
  return hero.value.media.logo.heights.desktop || '32px'
}

onMounted(() => {
  checkScreenSize()
  window.addEventListener('resize', checkScreenSize)
})

onUnmounted(() => {
  if (typeof window !== 'undefined') {
    window.removeEventListener('resize', checkScreenSize)
  }
})
</script>

<style scoped>
.logo-hero {
  height: 24px;
}
@media (min-width: 768px) {
  .logo-hero {
    height: 28px;
  }
}
@media (min-width: 1024px) {
  .logo-hero {
    height: 32px;
  }
}
</style>

<style scoped>
/* Custom styles if needed */
</style>

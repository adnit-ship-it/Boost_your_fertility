<template>
  <UiSectionWrapper class="py-20 md:py-32 relative bg-[#E3D5D4] text-bodyColor">
    <UiSectionContainer class="flex md:flex-row-reverse pb-8 md:pb-0">
      <div class="absolute bottom-0 right-0 z-20 max-h-[320px] md:max-h-[384px] lg:max-h-[480px]" 
        :style="{ height: getProductHeight() }">
        <img 
          :src="cta?.media?.product?.src || '/assets/images/products.png'" 
          :alt="cta?.media?.product?.alt || 'cta image'"
          class="h-full max-h-full max-w-full w-full object-contain" />
      </div>

      <div class="relative z-30 w-full h-full">
        <div class="flex flex-col h-full">
          <h2 v-motion :initial="{ opacity: 0, y: 32 }" :visible-once="{
            opacity: 1,
            y: 0,
            transition: {
              duration: 400,
              type: 'ease-in',
              stiffness: 250,
              damping: 25,
              mass: 1,
              delay: 100,
            },
          }" class="font-semibold text-[20px] md:text-[28px] lg:text-[32px]">
            {{ cta?.title || 'Real Results' }}
          </h2>
          <p v-motion :initial="{ opacity: 0, y: 32 }" :visible-once="{
            opacity: 1,
            y: 0,
            transition: {
              duration: 400,
              type: 'ease-in',
              stiffness: 250,
              damping: 25,
              mass: 1,
              delay: 150,
            },
          }" class="pt-5 text-[16px] md:text-[20px] lg:text-[28px] font-regular">
            {{ cta?.subtitle || 'See why Medivora is the #1 choice for GLP-1 Medications' }}
          </p>

          <div class="flex flex-col lg:flex-row gap-2 lg:gap-7 pt-4 lg:pt-5">
            <UiButton v-motion :initial="{ opacity: 0, y: 32 }" :visible-once="{
              opacity: 1,
              y: 0,
              transition: {
                duration: 400,
                type: 'ease-in',
                stiffness: 250,
                damping: 25,
                mass: 1,
                delay: 200,
              },
            }" ghost decorative :width="isMobile ? '145px' : '320px'" :height="isMobile ? '24px' : '44px'"
              :font-size="isMobile ? '12' : '24'">{{ cta?.stats.patients || '1000+ Patients' }}</UiButton>
            <UiButton v-motion :initial="{ opacity: 0, y: 32 }" :visible-once="{
              opacity: 1,
              y: 0,
              transition: {
                duration: 400,
                type: 'ease-in',
                stiffness: 250,
                damping: 25,
                mass: 1,
                delay: 250,
              },
            }" ghost decorative :width="isMobile ? '200px' : '320px'" :height="isMobile ? '24px' : '44px'"
              :font-size="isMobile ? '14' : '24'">{{ cta?.stats.worldwideCare || 'Worldwide Care' }}</UiButton>
          </div>

          <div class="pt-7 lg:pt-[50px] flex flex-col lg:flex-row gap-2 lg:gap-10 font-medium">
            <div 
              v-for="(feature, index) in firstHalfFeatures" 
              :key="index"
              v-motion 
              :initial="{ opacity: 0, y: 32 }" 
              :visible-once="{
                opacity: 1,
                y: 0,
                transition: {
                  duration: 400,
                  type: 'ease-in',
                  stiffness: 250,
                  damping: 25,
                  mass: 1,
                  delay: 300,
                },
              }" 
              class="flex gap-1 items-center text-[12px] md:text-[20px] lg:text-[24px]"
              :class="{ 'font-medium': index === 0 }"
            >
              <UiIcon 
                :icon-type="feature.iconType || 'checkmark-star'"
                :icon-color="feature.iconColor || '#AA992C'"
                :size="isMobile ? '18px' : isTablet ? '24px' : '28px'"
                alt="star icon"
              />
              <p>{{ feature.text }}</p>
            </div>
          </div>
          <div v-if="secondHalfFeatures.length > 0" class="mt-2 lg:mt-9 flex flex-col lg:flex-row gap-2 lg:gap-10">
            <div 
              v-for="(feature, index) in secondHalfFeatures" 
              :key="index"
              v-motion 
              :initial="{ opacity: 0, y: 32 }" 
              :visible-once="{
                opacity: 1,
                y: 0,
                transition: {
                  duration: 400,
                  type: 'ease-in',
                  stiffness: 250,
                  damping: 25,
                  mass: 1,
                  delay: 300,
                },
              }" 
              class="flex gap-1 items-center text-[12px] md:text-[20px] lg:text-[24px]"
            >
              <UiIcon 
                :icon-type="feature.iconType || 'checkmark-star'"
                :icon-color="feature.iconColor || '#AA992C'"
                :size="isMobile ? '18px' : isTablet ? '24px' : '28px'"
                alt="star icon"
              />
              <p>{{ feature.text }}</p>
            </div>
          </div>
        </div>
      </div>
    </UiSectionContainer>
  </UiSectionWrapper>


</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from "vue";
import { useSiteTextStore } from '~/stores/siteText';

const siteTextStore = useSiteTextStore();

// Initialize site text once for this component
const cta = computed(() => siteTextStore.getHomeText()?.cta);

const isMobile = ref(false);
const isTablet = ref(false);

const checkScreenSize = () => {
  if (typeof window !== 'undefined') {
    isMobile.value = window.innerWidth <= 768;
    isTablet.value = window.innerWidth > 768 && window.innerWidth < 1024;
  }
};

const features = computed(() => {
  return cta.value?.features || []
});

const firstHalfFeatures = computed(() => {
  const mid = Math.ceil(features.value.length / 2);
  return features.value.slice(0, mid);
});

const secondHalfFeatures = computed(() => {
  const mid = Math.ceil(features.value.length / 2);
  return features.value.slice(mid);
});

const getProductHeight = () => {
  if (!cta.value?.media?.product?.heights) return '222px';
  if (isMobile.value) return cta.value.media.product.heights.mobile || '222px';
  if (cta.value.media.product.heights.desktop === 'auto') return 'auto';
  return cta.value.media.product.heights.desktop || 'auto';
};

onMounted(() => {
  checkScreenSize();
  window.addEventListener("resize", checkScreenSize);
});

onUnmounted(() => {
  if (typeof window !== 'undefined') {
    window.removeEventListener("resize", checkScreenSize);
  }
});
</script>

<style scoped>
/* Component styles can be added here */
</style>

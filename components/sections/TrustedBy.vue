<template>
  <div v-if="trustedBy?.show !== false" class="mx-auto max-w-screen overflow-x-hidden pt-[60px]">
    <!-- Heading -->
    <h2 :class="[
      'section-title text-bodyColor mb-2 md:mb-8',
      center ? 'text-center' : 'pl-4 md:pl-20',
    ]">
      {{ trustedBy?.title || 'Trusted By' }}
    </h2>

 

    <!-- Mobile & Tablet: Marquee -->
    <div class="h-[96px] w-full border-y border-[#D9D9D9] overflow-hidden">
      <NuxtMarquee :speed="trustedBy?.marquee?.speed || 20" :pause-on-hover="true" class="h-full">
        <div class="flex items-center gap-8 lg:gap-16 h-full px-4">
          <template v-for="(logo, index) in trustedBy?.logos" :key="index">
            <img 
              v-for="n in 2" 
              :key="`${index}-${n}`"
              :src="logo.src" 
              :alt="logo.alt" 
              class="h-8 lg:h-10 w-auto object-contain" 
            />
          </template>
        </div>
      </NuxtMarquee>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useSiteTextStore } from '~/stores/siteText'

// Define the center prop with a default value of false
const props = defineProps({
  center: {
    type: Boolean,
    default: false,
  },
})

const siteTextStore = useSiteTextStore()
const trustedBy = computed(() => siteTextStore.getHomeText()?.trustedBy)
</script>

<style scoped>
/* Custom styles if needed */
</style>

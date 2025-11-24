<template>
  <footer
    class="bg-accentColor1 text-white flex justify-center pb-4 footer-height"
    :style="{ height: getFooterHeight() }"
  >
    <div
      class="max-w-[1328px] w-full flex items-end justify-between md:justify-center px-4 md:px-8 md:gap-8"
    >
      <div class="flex items-center">
        <NuxtLink to="/">
          <div class="pb-1" :style="{ height: getLogoHeight() }">
            <img
              src="/assets/images/brand/logo-alt.svg"
              :alt="common?.accessibility?.brandLogo || 'Brand Logo'"
              class="h-full w-full"
            />
          </div>
        </NuxtLink>
      </div>
      <div class="md:block h-[1px] mb-1.5 w-full mx-2 md:mx-5 flex-1 bg-white"></div>

      <!-- Navigation buttons on the right -->
      <div class="flex items-center gap-x-2 md:gap-x-6">
        <NuxtLink
          to="/about"
          class="text-white text-[14px] md:text-[18px] lg:text-[20px] transition-colors duration-200"
        >
          {{ common?.navigation?.about || 'About' }}
        </NuxtLink>
        <NuxtLink
          to="/contact"
          class="text-white text-[14px] md:text-[18px] lg:text-[20px] transition-colors duration-200"
        >
          {{ common?.navigation?.contactUs || 'Contact Us' }}
        </NuxtLink>
        <NuxtLink
          to="/products"
          class="text-white text-[14px] md:text-[18px] lg:text-[20px] transition-colors duration-200"
        >
          {{ common?.navigation?.products || 'Products' }}
        </NuxtLink>
      </div>
    </div>
    <!-- Logo on the left -->
  </footer>
</template>

<script setup>
import { computed, ref, onMounted, onUnmounted } from "vue";
import { useSiteTextStore } from "~/stores/siteText";

const siteTextStore = useSiteTextStore();
const common = computed(() => siteTextStore.getCommonText());
const isMobile = ref(false);

const checkScreenSize = () => {
  if (typeof window !== 'undefined') {
    isMobile.value = window.innerWidth < 1024;
  }
};

const getFooterHeight = () => {
  if (!common.value?.layout?.footer?.height) return '64px';
  return isMobile.value 
    ? common.value.layout.footer.height.mobile || '64px'
    : common.value.layout.footer.height.desktop || '72px';
};

const getLogoHeight = () => {
  if (!common.value?.layout?.footer?.logo?.height) return '20px';
  return isMobile.value
    ? common.value.layout.footer.logo.height.mobile || '20px'
    : common.value.layout.footer.logo.height.desktop || '32px';
};

onMounted(() => {
  checkScreenSize();
  window.addEventListener('resize', checkScreenSize);
});

onUnmounted(() => {
  if (typeof window !== 'undefined') {
    window.removeEventListener('resize', checkScreenSize);
  }
});
</script>

<style scoped>
/* Custom styles if needed */
</style>

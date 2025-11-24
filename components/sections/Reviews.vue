<template>
  <UiSectionWrapper class="flex-col py-20">
    <UiSectionContainer>
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
      }" class="pb-[24px] font-defaultSerif text-[20px] md:text-[28px] lg:text-[32px] font-semibold text-black">
        {{ reviewsData?.title || 'What Our Customers Have To Say' }}
      </h2>
    </UiSectionContainer>


    <div class="w-full flex flex-col gap-5 md:gap-8">
      <NuxtMarquee  v-motion :initial="{ opacity: 0, y: 32 }" :visible-once="{
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
      }"  :speed="reviewsData?.marquee?.speed || 50" :autoFill="true" class="flex gap-8">
        <div class="flex gap-5 md:gap-8">
          <div v-for="review in topReviews" :key="`top-${review.name}`"
            class="min-w-[320px] md:min-w-[492px] max-w-[320px] md:max-w-[492px] h-[136px] md:h-[188px] border-accentColor1 border rounded-[16px] md:rounded-[28px] bg-backgroundColor p-2 md:p-4 flex flex-col gap-5 md:gap-8">
            <div class="flex items-center justify-between">
              <div class="flex gap-[6px] items-center">
                <UiIcon 
                  :icon-src="avatarSrc"
                  :icon-color="avatarColor"
                  :size="isMobile ? '24px' : '32px'"
                  alt="user icon"
                />
                <p class="text-accentColor1 text-[16px] md:text-[24px] font-medium">
                  {{ review.name }}
                </p>
              </div>
              <div class="flex gap-1.5 md:gap-3">
                <UiIcon 
                  v-for="star in review.stars" 
                  :key="star" 
                  :icon-src="starSrc"
                  :icon-color="starColor"
                  :size="isMobile ? '22px' : '28px'"
                  alt="star icon"
                />
              </div>
            </div>
            <p class="text-[16px] md:text-[24px] text-center leading-tight px-2"
              style="white-space: normal !important; word-wrap: break-word !important;">
              "{{ review.review }}"
            </p>
          </div>
        </div>
      </NuxtMarquee>

      <NuxtMarquee  v-motion :initial="{ opacity: 0, y: 32 }" :visible-once="{
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
      }"  :speed="reviewsData?.marquee?.speed || 50" :direction="'right'" :autoFill="true" class="flex gap-8">
        <div class="flex gap-5 md:gap-8">
          <div v-for="review in bottomReviews" :key="`bottom-${review.name}`"
            class="min-w-[320px] md:min-w-[492px] max-w-[320px] md:max-w-[492px] h-[136px] md:h-[188px] border-accentColor1 border rounded-[16px] md:rounded-[28px] bg-backgroundColor p-2 md:p-4 flex flex-col gap-5 md:gap-8">
            <div class="flex items-center justify-between">
              <div class="flex gap-[6px] items-center">
                <UiIcon 
                  :icon-src="avatarSrc"
                  :icon-color="avatarColor"
                  :size="isMobile ? '24px' : '32px'"
                  alt="user icon"
                />
                <p class="text-accentColor1 text-[16px] md:text-[24px] font-medium">
                  {{ review.name }}
                </p>
              </div>
              <div class="flex gap-1.5 md:gap-3">
                <UiIcon 
                  v-for="star in review.stars" 
                  :key="star" 
                  :icon-src="starSrc"
                  :icon-color="starColor"
                  :size="isMobile ? '22px' : '28px'"
                  alt="star icon"
                />
              </div>
            </div>
            <p class="text-[16px] md:text-[24px] text-center leading-tight px-2"
              style="white-space: normal !important; word-wrap: break-word !important;">
              "{{ review.review }}"
            </p>
          </div>
        </div>
      </NuxtMarquee>
    </div>
  </UiSectionWrapper>

</template>

<script setup>
import { computed, ref, onMounted, onUnmounted } from 'vue'
import { useSiteTextStore } from '~/stores/siteText'

const siteTextStore = useSiteTextStore()
const reviewsData = computed(() => siteTextStore.getHomeText()?.reviews)

// Use reviews from websiteText.json, fallback to empty array
const reviews = computed(() => reviewsData.value?.list || [])

// Split reviews into two arrays for top and bottom rows
const topReviews = computed(() => reviews.value.slice(0, Math.ceil(reviews.value.length / 2)))
const bottomReviews = computed(() => reviews.value.slice(Math.ceil(reviews.value.length / 2)))

const avatarColor = computed(() => reviewsData.value?.media?.avatar?.color || '#A75809')
const starColor = computed(() => reviewsData.value?.media?.star?.color || '#D3C984')
const avatarSrc = computed(() => reviewsData.value?.media?.avatar?.src || '/assets/images/user.svg')
const starSrc = computed(() => reviewsData.value?.media?.star?.src || '/assets/images/star.svg')

const isMobile = ref(false)
const checkMobile = () => {
  if (typeof window !== 'undefined') {
    isMobile.value = window.innerWidth <= 768
  }
}

onMounted(() => {
  checkMobile()
  window.addEventListener('resize', checkMobile)
})

onUnmounted(() => {
  if (typeof window !== 'undefined') {
    window.removeEventListener('resize', checkMobile)
  }
})
</script>

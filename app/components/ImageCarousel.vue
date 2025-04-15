<template>
    <div class="relative w-full mx-auto min-h-[250px] group">
        <!-- Global Skeleton Placeholder -->
        <div v-if="!allLoaded" class="absolute inset-0 bg-gray-300 animate-pulse rounded-lg z-10"></div>

        <!-- Carousel -->
        <Splide v-show="allLoaded" :options="options" class="w-full min-h-[250px]">
            <SplideSlide v-for="(img, i) in images" :key="i" class="w-full min-h-[250px]">
                <img :data-splide-lazy="img" :alt="'Slide ' + (i + 1)" class="w-full h-full object-cover rounded-lg"
                    @load="onImageLoad(i)" />
            </SplideSlide>
        </Splide>
    </div>
</template>

<script setup>
import { Splide, SplideSlide } from '@splidejs/vue-splide'
import '@splidejs/splide/dist/css/splide.min.css'
import { ref, computed } from 'vue'

const images = [
    '/images/slide1.webp',
    '/images/slide2.webp',
    '/images/slide3.webp',
]

const loaded = ref(images.map(() => false))

const onImageLoad = (index) => {
    loaded.value[index] = true
}

// Compute when all images are loaded
const allLoaded = computed(() => loaded.value.every(v => v))

const options = {
    type: 'loop',
    perPage: 1,
    autoplay: true,
    interval: 3000,
    arrows: true,
    pagination: true,
    lazyLoad: 'sequential',
}
</script>

<style lang="postcss">
.animate-pulse {
    animation: pulse 1.5s infinite;
}

@keyframes pulse {

    0%,
    100% {
        opacity: 1
    }

    50% {
        opacity: 0.5
    }
}

/* Hide Splide arrows by default */
.splide__arrow {
    @apply opacity-0 transition-opacity duration-300;
}

/* Show arrows when hovering over the parent .group */
.group:hover .splide__arrow {
    @apply opacity-100;
}
</style>
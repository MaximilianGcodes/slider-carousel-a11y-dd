<script setup>
import {ref, onMounted} from 'vue'
import EmblaCarousel from 'embla-carousel'

const viewport = ref(null)
const embla = ref(null)
const selectedIndex = ref(0)


const slides = [
  {
    image: 'https://picsum.photos/seed/slide1/600/400',
    alt: 'Alttext Slide 1',
    text: 'Slide 1 Inhalt'
  },
  {
    image: 'https://picsum.photos/seed/slide2/600/400',
    alt: 'Alttext Slide 2',
    text: 'Slide 2 Inhalt'
  },
  {
    image: 'https://picsum.photos/seed/slide3/600/400',
    alt: 'Alttext Slide 3',
    text: 'Slide 3 Inhalt'
  }
]


const scrollPrev = () => embla.value?.scrollPrev()
const scrollNext = () => embla.value?.scrollNext()
const scrollTo = (index) => embla.value?.scrollTo(index)

const updateSelected = () => {
  if (!embla.value) return
  selectedIndex.value = embla.value.selectedScrollSnap()
}

onMounted(() => {
  embla.value = EmblaCarousel(viewport.value, {
    loop: true,
    skipSnaps: false
  })
  embla.value.on('select', updateSelected)
  updateSelected()
})

</script>

<template>
  <section
      class="relative max-w-4xl mx-auto"
      role="region"
      aria-label="Bilderkarussell mit 3 Slides"
  >
    <p
        class="sr-only"
        aria-live="polite"
        aria-atomic="true"
        v-if="selectedIndex !== null"
        :key="'slide-' + selectedIndex"
    >
      Aktueller Slide: {{ selectedIndex + 1 }} von {{ slides.length }}
    </p>


    <!-- Navigation -->
    <div class="absolute inset-y-0 left-0 flex items-center z-10">
      <button
          @click="scrollPrev"
          class="bg-white border shadow p-2 rounded-full hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-blue-600"
          aria-label="Vorheriger Slide"
      >
        ◀
      </button>
    </div>
    <div class="absolute inset-y-0 right-0 flex items-center z-10">
      <button
          @click="scrollNext"
          class="bg-white border shadow p-2 rounded-full hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-blue-600"
          aria-label="Nächster Slide"
      >
        ▶
      </button>
    </div>

    <!-- Viewport -->
    <div class="overflow-hidden rounded-lg" ref="viewport">

      <div class="flex touch-pan-y will-change-transform" ref="container">

        <div
            v-for="(slide, index) in slides"
            :key="index"
            class="min-w-full shrink-0 px-2 snap-start"
            role="group"
            :aria-label="`Slide ${index + 1} von ${slides.length}`"
            :aria-hidden="selectedIndex !== index"
        >
          <div class="bg-white shadow p-6 rounded text-center">
            <img
                :src="slide.image"
                :alt="slide.alt"
                class="rounded mx-auto mb-4"
            >
            <p class="text-lg text-gray-800 mb-2">{{ slide.text }}</p>
            <button
                class="bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700"
                :tabindex="selectedIndex === index ? 0 : -1"
            >
              Mehr erfahren
            </button>
          </div>
        </div>

      </div>

    </div>



    <!-- Pagination -->
    <div class="flex justify-center mt-4 gap-2">
      <button
          v-for="(slide, index) in slides"
          :key="index"
          @click="scrollTo(index)"
          :tabindex="0"
          :class="[
    'w-3 h-3 rounded-full transition-all',
    selectedIndex === index ? 'bg-blue-600' : 'bg-gray-300 hover:bg-gray-500'
  ]"
          :aria-label="`Slide ${index + 1} von ${slides.length}`"
          :aria-current="selectedIndex === index ? 'true' : undefined"
      />

    </div>
  </section>
</template>

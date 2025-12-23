<template>
  <section id="section-1" class="scroll-toggle-section">
    <div class="content-wrapper">
      <div class="text-content">
        <h2 class="section-title">{{ currentContent.title }}</h2>
        <p class="section-description">{{ currentContent.description }}</p>
        <div class="page-indicator">{{ currentIndex + 1 }} / 3</div>
      </div>
      <div class="image-container">
        <img
          v-for="(image, index) in images"
          :key="image.id"
          :src="image.src"
          :alt="image.alt"
          :class="['section-image', { active: currentIndex === index }]"
          :style="{ zIndex: images.length - index }"
        />
      </div>
    </div>
  </section>
</template>

<script>
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

export default {
  name: 'SectionOne',
  props: {
    hoveredSection: {
      type: String,
      default: null,
    },
  },
 data() {
  return {
    currentIndex: 0,
    sectionContents: [
      {
        id: 'plan',
        title: 'Plan Your Project',
        description: 'Careful planning is the foundation of every successful project.',
      },
      {
        id: 'design',
        title: 'Design Your Vision',
        description: 'Bring your ideas to life with stunning and functional designs.',
      },
      {
        id: 'build',
        title: 'Build with Precision',
        description: 'Develop robust and scalable solutions.',
      },
    ],
    images: [
      {
         id: 'plan',
        src: require('@/assets/images/plan.jpg'),
        alt: 'Plan',
      },
      {
        id: 'design',
        src: require('@/assets/images/design.jpg'),
        alt: 'Design',
      },
      {
        id: 'build',
        src: require('@/assets/images/build.jpg'),
        alt: 'Build',
      },
    ],
    scrollTimeline: null,
  };
},

  computed: {
    currentContent() {
      return this.sectionContents[this.currentIndex];
    },
  },
  watch: {
    hoveredSection(newVal) {
      if (newVal) {
        const index = this.sectionContents.findIndex((item) => item.id === newVal);
        if (index !== -1 && index !== this.currentIndex) {
          this.animateContentChange(index);
        }
      } else {
  const scrollIndex = Math.min(
    Math.floor(this.scrollTimeline.scrollTrigger.progress * this.sectionContents.length),
    this.sectionContents.length - 1
  );

  if (scrollIndex !== this.currentIndex) {
    this.animateContentChange(scrollIndex);
  }
}

    },
  },
  mounted() {
    this.setupScrollTrigger();
  },
  methods: {
    setupScrollTrigger() {
      const section = this.$el;
      const numStates = this.sectionContents.length;

      this.scrollTimeline = gsap.timeline({
        scrollTrigger: {
          trigger: section,
          pin: true,
          scrub: true,
          start: 'top top',
          end: `+=${section.offsetHeight * (numStates - 1)}`,
          onUpdate: (self) => {
            const progress = self.progress;
            const newIndex = Math.min(
              Math.floor(progress * numStates),
              numStates - 1
            );
            if (newIndex !== this.currentIndex) {
              this.animateContentChange(newIndex);
            }
          },
        },
      });
    },
    animateContentChange(newIndex) {
      const oldIndex = this.currentIndex;
      this.currentIndex = newIndex;

      gsap.fromTo(
        this.$el.querySelector('.text-content h2'),
        { opacity: 0, y: 20 },
        { opacity: 1, y: 0, duration: 0.5, ease: 'power2.out' }
      );
      gsap.fromTo(
        this.$el.querySelector('.text-content p'),
        { opacity: 0, y: 20 },
        { opacity: 1, y: 0, duration: 0.5, ease: 'power2.out', delay: 0.1 }
      );
      gsap.fromTo(
        this.$el.querySelector('.page-indicator'),
        { opacity: 0, y: 20 },
        { opacity: 1, y: 0, duration: 0.5, ease: 'power2.out', delay: 0.2 }
      );

      const oldImage = this.$el.querySelectorAll('.section-image')[oldIndex];
      const newImage = this.$el.querySelectorAll('.section-image')[newIndex];

      if (oldImage && newImage) {
        const imageTimeline = gsap.timeline();

        imageTimeline
          .to(oldImage, { opacity: 0, scale: 1, duration: 0.5, ease: 'power2.out' }, 0)
          .fromTo(
            newImage,
            { opacity: 0, scale: 1.05 },
            { opacity: 1, scale: 1, duration: 0.5, ease: 'power2.out' },
            0
          );
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../assets/styles/section-1.scss';
</style>

<template>
  <section id="section-2" class="scroll-toggle-section">
    <div class="content-wrapper">
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
      <div class="text-content">
        <h2 class="section-title">{{ currentContent.title }}</h2>
        <p class="section-description">{{ currentContent.description }}</p>
        <div class="page-indicator">{{ currentIndex + 1 }} / {{ sectionContents.length }}</div>
      </div>
    </div>
  </section>
</template>

<script>
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

export default {
  name: 'SectionTwo',
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
          end: '+=150%', // Limited end as per requirements
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

      this.images.forEach((image, index) => {
        gsap.set(this.$el.querySelectorAll('.section-image')[index], {
          opacity: index === 0 ? 1 : 0,
          scale: index === 0 ? 1 : 1.3,
          willChange: 'transform, opacity',
        });
      });

      gsap.set(this.$el.querySelector('.text-content h2'), { opacity: 1, willChange: 'opacity, transform' });
      gsap.set(this.$el.querySelector('.text-content p'), { opacity: 1, willChange: 'opacity, transform' });
      gsap.set(this.$el.querySelector('.page-indicator'), { opacity: 1, willChange: 'opacity, transform' });
    },
    animateContentChange(newIndex) {
      const oldIndex = this.currentIndex;
      this.currentIndex = newIndex;

      gsap.to(this.$el.querySelector('.text-content h2'), { opacity: 0, duration: 0.2, ease: 'power1.out' });
      gsap.to(this.$el.querySelector('.text-content p'), { opacity: 0, duration: 0.2, ease: 'power1.out' });
      gsap.to(this.$el.querySelector('.page-indicator'), { opacity: 0, duration: 0.2, ease: 'power1.out' });

      gsap.to(this.$el.querySelector('.text-content h2'), { opacity: 1, duration: 0.2, ease: 'power1.in', delay: 0.2 });
      gsap.to(this.$el.querySelector('.text-content p'), { opacity: 1, duration: 0.2, ease: 'power1.in', delay: 0.2 });
      gsap.to(this.$el.querySelector('.page-indicator'), { opacity: 1, duration: 0.2, ease: 'power1.in', delay: 0.2 });


      const oldImage = this.$el.querySelectorAll('.section-image')[oldIndex];
      const newImage = this.$el.querySelectorAll('.section-image')[newIndex];

      if (oldImage && newImage) {
        gsap.to(oldImage, { opacity: 0, scale: 1.3, duration: 0.5, ease: 'power2.out' });
        gsap.fromTo(
          newImage,
          { opacity: 0, scale: 1.3 },
          { opacity: 1, scale: 1, duration: 0.5, ease: 'power2.out' }
        );
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../assets/styles/section-2.scss';
</style>

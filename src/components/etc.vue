<template>
  <section id="etc" class="etc-section">
    <div class="content-wrapper">
      <h2>Thank You</h2>
      <p>
        Discover more about our services and how we can help you achieve your digital goals.
        Contact us today for a consultation.
      </p>
    </div>
  </section>
</template>

<script>
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

export default {
  name: 'EtcSection',
  mounted() {
    gsap.from(this.$el.querySelector('.content-wrapper'), {
      scrollTrigger: {
        trigger: this.$el,
        start: 'top center',
        toggleActions: 'play none none reverse',
      },
      opacity: 0,
      y: 50,
      duration: 1,
      ease: 'power2.out',
    });
  },
};
</script>

<style lang="scss" scoped>
@import '@/assets/styles/etc.scss';
</style>

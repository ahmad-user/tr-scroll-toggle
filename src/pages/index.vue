<template>
  <div class="home-page">
    <HeaderComponent @hover-section="handleHoverSection" :activeSection="activeSection" />
    <Section1 :hoveredSection="hoveredSection" />
    <Section2 />
    <EtcSection />
  </div>
</template>

<script>
import HeaderComponent from '@/components/header.vue';
import Section1 from '@/components/section-1.vue';
import Section2 from '@/components/section-2.vue';
import EtcSection from '@/components/etc.vue';

export default {
  name: 'HomePage',
  components: {
    HeaderComponent,
    Section1,
    Section2,
    EtcSection,
  },
  data() {
    return {
      activeSection: 'plan',
      hoveredSection: null,
    };
  },
  methods: {
    handleHoverSection(sectionId) {
      this.hoveredSection = sectionId;
    },
  },
};
</script>

<style lang="scss" scoped>
.home-page {
  // Global styles for the home page if needed
}
</style>

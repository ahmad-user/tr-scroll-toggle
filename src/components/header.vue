<template>
  <header class="header">
    <nav class="nav">
      <ul>
        <li
          v-for="(item, index) in navItems"
          :key="index"
          :class="{ active: activeSection === item.id }"
          @mouseover="handleMouseOver(item.id)"
          @mouseleave="handleMouseLeave()"
          @click="scrollToSection(item.id)"
        >
          {{ item.label }}
        </li>
      </ul>
    </nav>
  </header>
</template>

<script>
import { gsap } from 'gsap';

export default {
  name: 'HeaderComponent',
  props: {
    activeSection: {
      type: String,
      default: 'plan',
    },
  },
  data() {
    return {
      navItems: [
        { id: 'plan', label: 'Plan' },
        { id: 'design', label: 'Design' },
        { id: 'build', label: 'Build' },
      ],
      hoveredSection: null,
    };
  },
  methods: {
    handleMouseOver(sectionId) {
      this.hoveredSection = sectionId;
      this.$emit('hover-section', sectionId);
    },
    handleMouseLeave() {
      this.hoveredSection = null;
      this.$emit('hover-section', null);
    },
    scrollToSection(sectionId) {
      const section = document.getElementById(sectionId);
      if (section) {
        gsap.to(window, {
          duration: 1,
          scrollTo: {
            y: section,
            offsetY: 0, 
          },
          ease: 'power2.inOut',
        });
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../assets/styles/header.scss';
</style>

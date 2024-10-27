<template>
  <div class="container">
    <div class="animation-container" ref="lottieContainer"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import lottie from 'lottie-web';
import gsap from 'gsap';
import ScrollTrigger from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

const lottieContainer = ref(null);
let animation = null;
let scrollTrigger = null;

onMounted(() => {
  // Wait for both DOM and Lottie animation to be ready
  const initializeAnimation = () => {
    animation = lottie.loadAnimation({
      container: lottieContainer.value,
      renderer: 'svg',
      loop: false,
      autoplay: false,
      path: '/danceHover7.json',
    });

    // Wait for animation to be loaded before setting up ScrollTrigger
    animation.addEventListener('DOMLoaded', () => {
      // Clear existing ScrollTrigger if it exists
      if (scrollTrigger) {
        scrollTrigger.kill();
      }

      // Set up new ScrollTrigger
      scrollTrigger = ScrollTrigger.create({
        trigger: lottieContainer.value,
        start: "center center",
        end: "200% bottom",
        scrub: true,
        markers: true,
        onUpdate: (self) => {
          const frame = Math.round(self.progress * (animation.totalFrames - 1));
          animation.goToAndStop(frame, true);
        },
        onRefresh: () => {
          // Ensure animation is at correct frame after refresh
          animation.goToAndStop(0, true);
        }
      });

      // Add click functionality
      const clickableElement = lottieContainer.value.querySelector('.balls-id');
      if (clickableElement) {
        clickableElement.style.cursor = 'pointer';
        clickableElement.addEventListener('click', () => {
          window.open('https://www.samuelday.de/', '_blank');
        });
      }
    });
  };

  // Initialize animation
  initializeAnimation();

  // Handle window resize
  const handleResize = gsap.debounce(() => {
    // Refresh ScrollTrigger on resize
    ScrollTrigger.refresh();
  }, 250);

  window.addEventListener('resize', handleResize);

  // Cleanup function
  onBeforeUnmount(() => {
    if (scrollTrigger) {
      scrollTrigger.kill();
    }
    if (animation) {
      animation.destroy();
    }
    window.removeEventListener('resize', handleResize);
  });
});
</script>

<style>
/* Your existing styles remain the same */
.container {
  width: 100vw;
  height: 14.28vw;
  display: flex;
  justify-content: space-between;
  align-items: center;
  overflow: hidden;
  position: relative;
}

.animation-container {
  position: fixed;
  width: 100%;
  height: auto;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 10;
}
</style>
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

// Simple debounce function implementation
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}


onMounted(() => {
  // Detect if device is mobile
  const isMobile = /iPhone|iPad|iPod|Android/i.test(navigator.userAgent);

  const initializeAnimation = () => {
    animation = lottie.loadAnimation({
      container: lottieContainer.value,
      renderer: 'svg',
      loop: false,
      autoplay: false,
      path: '/finalJson.json',
    });

    animation.addEventListener('DOMLoaded', () => {
      if (scrollTrigger) {
        scrollTrigger.kill();
      }

      scrollTrigger = ScrollTrigger.create({
        trigger: lottieContainer.value,
        start: "center center",
        end: "600% bottom",
        scrub: isMobile ? 0.5 : true,
        markers: true,
        onUpdate: (self) => {
          requestAnimationFrame(() => {
            const frame = Math.round(self.progress * (animation.totalFrames - 1));
            animation.goToAndStop(frame, true);
          });
        }
      });
    });
  };


  initializeAnimation();

// Optimized resize handler using our debounce function
const handleResize = debounce(() => {
  ScrollTrigger.refresh(true); // Force refresh
}, isMobile ? 500 : 250);

window.addEventListener('resize', handleResize);

// Handle orientation changes
if (isMobile) {
  window.addEventListener('orientationchange', () => {
    setTimeout(() => {
      ScrollTrigger.refresh(true);
    }, 10);
  });
}

onBeforeUnmount(() => {
  if (scrollTrigger) {
    scrollTrigger.kill();
  }
  if (animation) {
    animation.destroy();
  }
  window.removeEventListener('resize', handleResize);
  if (isMobile) {
    window.removeEventListener('orientationchange', handleResize);
  }
});
});
</script>


<style >
.container {
  width: 100vw;
  /* Full width of the viewport */
  height: 14.28vw;
  /* width/7 to keep the scroll timing intact. */
  /* max-height: 300vh;         Limit to full viewport height */
  display: flex;
  /* Use flexbox to center the animation */
  justify-content: space-between;
  /* Center horizontally */
  align-items: center;
  /* Center vertically */
  overflow: hidden;
  /* Prevent overflow of content */
  position: relative;
}

.animation-container {
  position: fixed;
  width: 100%;
  /* Adjust to keep some margin */
  height: auto;
  /* Adjust based on your needs */
  top: 50%;
  /* Center it vertically */
  left: 50%;
  /* Center it horizontally */
  transform: translate(-50%, -50%);
  z-index: 10;


}

@media only screen and (min-width:600px) {
  .inside-the-mind-mobile {
    display: none !important;
  }

  .scroll-header {
    display: none !important;
  }

  .click-to-view {
    display: none !important;
  }

  .allhands-window {
    display: none !important;
  }

  .balls-id path {
    /* display: none !important; */
    fill: aqua !important;
    background-color: blue !important;
  }

  .hello-desktop {

    display: none !important;

  }

}

@media only screen and (max-width:600px) {
  .inside-the-mind {
    display: none !important;
  }

  .hello-desktop {

    display: none !important;

  }

 

}

html{
  scroll-behavior: smooth;
}
/* 
.text-precomp {

animation: float 3s ease-in-out infinite !important;
display: inline-block !important;
/* opacity: 0.5; */

.smartframe {
    position: relative; /* Ensure positioned elements are relative to this container */
}

.superman {
    position: relative; /* Ensure positioned elements are relative to this container */
}

.frame1,
.frame2,
.superman1,
.superman2 {
    position: absolute; /* Position them absolutely within the smartframe */
}

.frame1,
.superman1 {
    animation: showFrame1 0.3s infinite; /* Set animation for frame1 */
}

.frame2,
.superman2 {
    opacity: 0; /* Initially hidden */
    animation: showFrame2 0.3s infinite; /* Set animation for frame2 */
}

@keyframes showFrame1 {
    0%, 50% {
        opacity: 1; /* Fully visible for the first half */
    }
    50.1%, 100% {
        opacity: 0; /* Fade out during the second half */
    }
}

@keyframes showFrame2 {
    0%, 50% {
        opacity: 0; /* Initially hidden for the first half */
    }
    50.1%, 100% {
        opacity: 1; /* Fully visible during the second half */
    }
}



/* Standard keyframes */
@keyframes float {
  0% {
    transform: translateY(0%);
  }

  50% {
    transform: translateY(80px);
  }

  100% {
    transform: translateY(0%);
  }
}

/* WebKit keyframes */
@-webkit-keyframes float {
  0% {
    -webkit-transform: translateY(0);
  }

  50% {
    -webkit-transform: translateY(-80px);
  }

  100% {
    -webkit-transform: translateY(0);
  }
}

@-webkit-keyframes noise {

  0%,
  100% {
    background-position: 0% 0%;
  }

  10% {
    background-position: -5% -10%;
  }

  20% {
    background-position: -15% 5%;
  }

  30% {
    background-position: 7% -25%;
  }

  40% {
    background-position: 20% 25%;
  }

  50% {
    background-position: -25% 10%;
  }

  60% {
    background-position: 15% 5%;
  }

  70% {
    background-position: 0% 15%;
  }

  80% {
    background-position: 25% 35%;
  }

  90% {
    background-position: -10% 10%;
  }
}

@keyframes noise {

  0%,
  100% {
    background-position: 0% 0%;
  }

  10% {
    background-position: -5% -10%;
  }

  20% {
    background-position: -15% 5%;
  }

  30% {
    background-position: 7% -25%;
  }

  40% {
    background-position: 20% 25%;
  }

  50% {
    background-position: -25% 10%;
  }

  60% {
    background-position: 15% 5%;
  }

  70% {
    background-position: 0% 15%;
  }

  80% {
    background-position: 25% 35%;
  }

  90% {
    background-position: -10% 10%;
  }
}

/* Noise effect applied to the entire HTML */
body::before {
  content: "";
  z-index: 9999;
  /* Ensure it's on top */
  position: fixed;
  /* Change to fixed */
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  /* background: url("@/assets/blacktexture.avif"); */
  background: url(https://herdl.com/wp-content/uploads/2020/11/noise-web.webp);
  /* background-size: cover; */
  background-repeat: repeat;
  /* Ensures the image tiles */
  background-size: auto;
  /* 
/* Change to cover for better appearance */
  animation: noise 100ms infinite;
  /* Increased duration */
  pointer-events: none;
  /* Allows interaction with other elements */
  opacity: 0.5;

  mix-blend-mode: multiply;
}
</style>
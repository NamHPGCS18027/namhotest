<template>
    <div class="slideshow">
      <div class="slideshow-image-container">
        <img :src="images[currentIndex]" alt="Slideshow Image" class="slideshow-image" />
      </div>
      <div class="slideshow-controls">
        <button @click="prevImage">Trước</button>
        <button @click="nextImage">Sau</button>
      </div>
      <div class="slideshow-indicators">
        <span
          v-for="(image, index) in images"
          :key="index"
          :class="['indicator', { active: index === currentIndex }]"
          @click="goToImage(index)"
        ></span>
      </div>
    </div>
  </template>
  
  <script>
  import slider1 from '../assets/slider1.jpg'
  import slider2 from '../assets/slider2.jpg'
  import slider3 from '../assets/slider3.jpg'
  export default {
    data() {
      return {
        currentIndex: 0,
        images: [
        slider1,
        slider2,
        slider3,
        ],
      };
    },
    methods: {
      nextImage() {
        this.currentIndex = (this.currentIndex + 1) % this.images.length;
      },
      prevImage() {
        this.currentIndex =
          (this.currentIndex - 1 + this.images.length) % this.images.length;
      },
      goToImage(index) {
        this.currentIndex = index;
      },
    },
    mounted() {
      // Tự động chuyển ảnh sau mỗi 3 giây
      this.autoSlide = setInterval(this.nextImage, 3000);
    },
    beforeDestroy() {
      clearInterval(this.autoSlide);
    },
  };
  </script>
  
  <style scoped>
  .slideshow {
    width: 100%;
    max-width: 800px;
    margin: auto;
    text-align: center;
    position: relative;
  }
  .slideshow-image-container {
    position: relative;
    width: 100%;
    height: 210px;
    overflow: hidden;
  }
  .slideshow-image {
    width: 100%;
    height: auto;
    transition: opacity 0.5s ease;
  }
  .slideshow-controls {
    margin: 10px 0;
  }
  .slideshow-controls button {
    padding: 10px 20px;
    margin: 0 5px;
    font-size: 16px;
    cursor: pointer;
  }
  .slideshow-indicators {
    margin-top: 10px;
  }
  .indicator {
    display: inline-block;
    width: 10px;
    height: 10px;
    margin: 0 5px;
    border-radius: 50%;
    background-color: #ccc;
    cursor: pointer;
  }
  .indicator.active {
    background-color: #333;
  }
  </style>
  
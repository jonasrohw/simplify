<!-- <style scoped>

  /* .input-wrapper {
    @apply flex flex-col h-20 text-gray-300;
  }
  input[type=text], select {
    @apply bg-secondary border border-gray-700 text-gray-300 rounded-lg h-full mt-2 text-sm px-2 py-3;
  } */
  img {
    @apply w-full h-full rounded-lg;
  }
</style>
<template>
	<div class="fill">
		<img src="/long_story.png" alt="long story 1">
	</div>
  <Splide class="mt-3 z-10" :options="options">
    <SplideSlide>
      <img src="/banner-1.jpg" alt="Sample 1">
    </SplideSlide>
    <SplideSlide>
      <img src="/banner-2.jpg" alt="Sample 2">
    </SplideSlide>
    <SplideSlide>
      <img src="/banner-3.jpg" alt="Sample 1">
    </SplideSlide>
    <SplideSlide>
      <img src="/banner-2.jpg" alt="Sample 2">
    </SplideSlide>
  </Splide> -->
<!-- </template> -->

<template>
	<div
	  class="swipe-container"
	  @touchstart="onTouchStart"
	  @touchmove="onTouchMove"
	  @touchend="onTouchEnd"
	>
	  <img :src="currentImage" alt="Swipe Image" class="swipe-image" />
	</div>
  </template>
  
  <script>
  export default {
	name: 'SwipeImage',
	data() {
	  return {
		images: [
		'/banner-1.jpg', // Replace this with your actual image path
        '/long-story.png', // Replace this with your actual image path
        '/banner-2.jpg', // Replace this with your actual image path
      ],
		currentIndex: 0,
		startX: 0,
		endX: 0,
	  };
	},
	computed: {
	  currentImage() {
		return this.images[this.currentIndex];
	  },
	},
	methods: {
	  onTouchStart(event) {
		this.startX = event.touches[0].clientX;
	  },
	  onTouchMove(event) {
		this.endX = event.touches[0].clientX;
	  },
	  onTouchEnd() {
		if (this.endX - this.startX > 50 && this.currentIndex > 0) {
		  this.currentIndex--;
		} else if (this.startX - this.endX > 50 && this.currentIndex < this.images.length - 1) {
		  this.currentIndex++;
		}
		this.startX = 0;
		this.endX = 0;
	  },
	},
  };
  </script>
  
  <style>
  .swipe-container {
	position: relative;
	width: 100%;
	height: 300px;
	overflow: hidden;
  }
  
  .swipe-image {
	width: 100%;
	height: 100%;
	object-fit: cover;
  }
  </style>
  
<!-- <template>
  <main class="w-full flex flex-col items-center gap-3 text-center">
    <div @click="navigate" class="w-full rounded-circle h-20 active:scale-75 duration-300 grid place-items-center bg-yellow shadow text-success border-2 border-gray-800 rounded-circle px-3 py-1">
      <img src="/story1.png" class="rounded-circle" alt="story 1">
    </div>
    <small class="text-gray-300">{{ source.name }}</small>
  </main>
  <img src="/story1.png" class="rounded-circle" alt="..." />
</template> -->
<template>
	<div :class="['round-div', customClass]" @click="navigate" >
	  <img src="/story1.png" class="round-image" alt="story 1">
	</div>
  </template>

<script>
export default {
  name: 'RoundDiv',
  props: {
    customClass: {
      type: String,
      default: ''
    }
  }
};
</script>

<style>
.round-div {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  overflow: hidden;
  background-color: #3498db;
}

.round-image {
  /* width: 50%;
  height: 50%;
  object-fit: cover; */

  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 50%;
}
</style>

<script setup>

import { useRouter } from 'vue-router'

const router = useRouter()
const props = defineProps({
  source: {
    type: Object
  }
})

const navigate = () => {
  setTimeout(() => {
    router.push({ name: props.source.to })
  }, 300)
}

</script>

<template>
  <canvas ref="canvas"></canvas>
</template>


<script setup lang="ts">
import tesselatonSpritesheet from './assets/tesselationSpritesheet.png'

const width = 1600;
const height = 800;

onMounted(async() => {
	await nextTick();
  console.log("mounted");


  const canvas = ref<HTMLCanvasElement | null>(null);
  console.log(canvas.value);

  if (canvas.value) {
    loadCanvas(canvas.value);
  } else {
    console.log("no canvas");
  }

});


function loadCanvas(canvas: HTMLCanvasElement) {
	const ctx = canvas.getContext('2d');
	const img = new Image();
	img.hidden = true;
	img.src = tesselatonSpritesheet;
}

</script>


<style>
  canvas{
			border: 2px solid black;
			border-radius: 2px;
			background-color: white ;
		}

		#hidden{
		display: none ;
		}
</style>
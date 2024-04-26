<template>
	<div>
		<canvas ref="canvas"></canvas>
	</div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import tesselatonSpritesheet from './assets/tesselationSpritesheet.png';

type direction = 0 | 1 | 2 | 3

const canvas = ref<HTMLCanvasElement | null>(null);

const img = new Image();
img.src = tesselatonSpritesheet;

const numTilesX = 16
const numTilesY = 16

const tileWidth = 32
const tileHeight = 32
const numRows = img.height / tileHeight

const cw = numTilesX * tileWidth
const ch = numTilesY * tileHeight

console.log("rows", numRows)

onMounted(async () => {
	await new Promise<void>((res) => { img.onload = () => { res() } });
	if (canvas.value) {
		const ctx = canvas.value.getContext('2d');
		canvas.value.width = cw;
		canvas.value.height = ch;
		draw(ctx)
	} else {
		console.error('Canvas element not found');
	}
});

function draw(ctx: CanvasRenderingContext2D) {
	ctx.clearRect(0, 0, cw, ch);
	ctx.fillStyle = 'black';
	ctx.fillRect(0, 0, cw, ch);

	for(let y = 0; y < numTilesY; y++) {
		for(let x = 0; x < numTilesX; x++) {
			ctx.filter = 'hue-rotate('+(Math.floor(Math.random() * 360))+'deg)'
			drawTile(ctx, x, y, randDirection(), randTile());
		}
	}
}

function drawTile(ctx: CanvasRenderingContext2D, tileX: number, tileY: number, rot: direction,imgNum: number) {
	ctx.save();
	ctx.translate(tileWidth / 2, tileHeight / 2);
	ctx.translate(tileX * tileWidth, tileY * tileHeight);
	ctx.rotate(rot * Math.PI / 2);

	ctx.drawImage(img,0,tileHeight * imgNum, tileWidth, tileHeight, -tileWidth / 2, -tileHeight / 2, tileWidth, tileHeight);

	ctx.restore();
}

function randDirection() {
	return <direction>Math.floor(Math.random() * 4);
}

function randTile() {
	return Math.floor(Math.random() * numRows);
}
</script>

<style scoped>
canvas {
	border: 2px solid black;
	border-radius: 2px;
	background-color: white;
}
</style>

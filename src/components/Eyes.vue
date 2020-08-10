<template>
  <svg id="eyes" v-on:mousemove="onmousemove">
    <Eye
      v-for="(eye,index) in eyes"
      v-bind:x="eye.x"
      v-bind:y="eye.y"
      v-bind:sz="eye.sz"
      v-bind:color="eye.color"
      :mx=mx
      :my=my
      :key=index
    />
    <!-- <Eye x=100 y=200 sz=20 color="blue" mx=300 my=300 />
    <Eye x=300 y=400 sz=20 color="blue" mx=300 my=300 />
    <Eye x=500 y=600 sz=20 color="blue" mx=300 my=300 /> -->
  </svg>
</template>

<script>
import Eye from './Eye'

function randomColor() {
  let h = Math.random() * 360
  let s = Math.round(50 + Math.random() * 50)
  let l = Math.round(30 + Math.random() * 40)
  return `hsl(${h}, ${s}%, ${l}%)`
}

function eyeDistance(eye1, eye2) {
  let dx = eye1.x - eye2.x;
  let dy = eye1.y - eye2.y;
  return Math.sqrt((dx * dx) + (dy * dy))
}

function canPlaceEye(eyes, newEye) {
  return eyes.every(eye =>
    eyeDistance(eye, newEye) >= eye.sz + newEye.sz + 5
  )
}

function randomEye() {
  let ww = window.innerWidth;
  let wh = window.innerHeight;
  let sz = 20 + Math.random() * 60;
  let x = sz + Math.random() * (ww - 2 * sz);
  let y = sz + Math.random() * (wh - 2 * sz);
  let color = randomColor();
  return { x, y, sz, color };
}

function createEyes() {
  let eyes = [];
  [...Array.from({ length: 1000 })].forEach(_ => {
    let newEye = randomEye();
    if (canPlaceEye(eyes, newEye)) {
      eyes.push(newEye)
    }
  })
  return eyes;
}

export default {
  name: "Eyes",
  components: {
    Eye,
  },
  data: () => ({
    eyes: [],
    mx: 0,
    my: 0,
  }),
  mounted: function() {
		let ww = window.innerWidth;
    let wh = window.innerHeight;
		this.mx = Math.random() * ww;
		this.my = Math.random() * wh;
    this.eyes = createEyes();
  },
  methods: {
    onmousemove: function(event) {
      let svg = document.getElementById("eyes");
      let rect = svg.getBoundingClientRect()
      this.mx = event.pageX - rect.x;
      this.my = event.pageY - rect.y;
    }
  }
}
</script>

<style scoped>
  svg {
    width: 100vw;
    height: 100vh;
    display: block;
    background-color: #aaa;
  }
</style>

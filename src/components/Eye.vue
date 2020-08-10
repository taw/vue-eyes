<template>
  <g>
    <circle class="eye1" :cx=x :cy=y :r=sz />
    <circle class="eye2" :cx=rx :cy=ry :r=sz05 :style=eyestyle />
    <circle class="eye3" :cx=rx :cy=ry :r=sz02 />
  </g>
</template>

<script>
import Eye from './Eye'

export default {
  name: "Eye",
  props: ["x", "y", "sz", "color", "mx", "my"],
  computed: {
    sz02: function() {
      return this.sz * 0.2
    },
    sz05: function() {
      return this.sz * 0.5
    },
    eyestyle: function() {
      return `fill: ${this.color}`
    },
    max_eye_movement: function() {
      return 0.3 * this.sz;
    },
    dx: function() {
      if (this.mx !== null)
        return (this.mx - this.x);
      else
        return 0;
    },
    dy: function() {
      if (this.my !== null)
        return (this.my - this.y);
      else
        return 0;
    },
    dl: function() {
      return Math.max(0.000001, Math.sqrt(this.dx*this.dx + this.dy*this.dy));
    },
    displacement: function() {
      return Math.min(this.max_eye_movement, this.dl)
    },
    rx: function() {
      return +this.x + this.dx/this.dl * this.displacement
    },
    ry: function() {
      return +this.y + this.dy/this.dl * this.displacement
    },
  },
}
</script>

<style scoped>
  .eye1 {
    fill: white;
    stroke: black;
    stroke-width: 3px;
  }
  .eye2 {
    stroke: black;
    stroke-width: 1px;
  }
  .eye3 {
    fill: black;
  }
</style>

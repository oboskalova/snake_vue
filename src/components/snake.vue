<template>
  <div id="snake" :style="styler">
    <div style="display:none">{{ cords }}</div>
  </div>
</template>

<script>
export default {
  name: 'snake',
  props: ['x', 'y'],
  data () {
    return {
      cordX: 0,
      cordY: 0,
    }
  },
  methods: {
    recalcPosition () {
      console.log('пересчитано')
      this.cordX = parseInt(this.x)
      this.cordY = parseInt(this.y)
    }
  },
  computed: {
    styler () {
      return {
        left: this.cordX + 'px',
        top: this.cordY + 'px'
      }
    },
    cords () {
      this.recalcPosition()
      return { x: this.$props.x, y: this.$props.y }
    }
  }
}
</script>

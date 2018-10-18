<template>
  <div id="food" :style="styler">
    <div style="display:none">{{ cords }}</div>
  </div>
</template>

<script>
export default {
  name: 'food',
  props: ['xF', 'yF'],
  data () {
    return {
      cordXFood: 0,
      cordYFood: 0,
    }
  },
  methods: {
    recalcPosition () {
      console.log('пересчитано')
      this.cordXFood = parseInt(this.xF)
      this.cordYFood = parseInt(this.yF)
    }
  },
  computed: {
    styler () {
      return {
        left: this.cordXFood + 'px',
        top: this.cordYFood + 'px'
      }
    },
    cords () {
      this.recalcPosition()
      return { xF: this.$props.xF, yF: this.$props.yF }
    }
  }
}
</script>

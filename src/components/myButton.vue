<template>
  <button
    v-text="priority"
    v-drag
    class="myButton"
    :class="{ 'active': isMove }"
    :style="{ left: left + 'px', top: top + 'px' }"
  />
</template>

<script>
export default {
  name: 'myButton',
  data: () => ({
    isMove: false
  }),
  props: {
    priority: {
      type: Number,
      required: false,
      default: () => false
    },
    left: {
      type: Number,
      required: false,
      default: () => false
    },
    top: {
      type: Number,
      required: false,
      default: () => false
    }
  },

  directives: {
    drag: {
      bind: (el) => {
        let isMove = false,
            shiftX,
            shiftY

        el.addEventListener('mousedown', (e) => {
          isMove = true
          shiftX = e.pageX - el.offsetLeft
          shiftY = e.pageY - el.offsetTop
          el.style.zIndex = 1
          /* eslint-disable no-console */
          console.log(el.offsetLeft)
          /* eslint-disable no-console */
        })

        window.addEventListener('mousemove', (e) => {
          if (isMove) {
            el.style.left = e.pageX - shiftX + 'px'
            el.style.top = e.pageY - shiftY + 'px'
          }
        })

        el.addEventListener('mouseup', () => {
          isMove = false
          el.style.zIndex = 0
        })
      },

      inserted: (el) => {
        console.log(el.parentNode)
      }
    }
  }
}
</script>

<style lang="sass">
.myButton
  width: 300px
  height: 150px
  background: #fff
  border: 2px solid #16211E
  font-size: 14px
  position: absolute
  &.active
    z-index: 1
  &.green
    background: #68bc48
  &.red
    background: #ed2025
</style>

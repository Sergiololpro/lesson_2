<template>
  <button
    v-text="priority"
    v-drag
    class="myButton"
  />
</template>

<script>
export default {
  name: 'myButton',
  data: () => ({
    isMove: true
  }),
  props: {
    priority: {
      type: Number,
      required: false,
      default: () => false
    }
  },

  directives: {
    drag: {
      bind: (el, bin) => {
        bin
        let isMove = false,
            objectX,
            objectY
            
            // shiftY

        el.addEventListener('mousedown', (e) => {
          isMove = true
          objectX = e.pageX
          objectY = e.pageY
          /* eslint-disable no-console */
          console.log(isMove)
          /* eslint-disable no-console */
        })

        window.addEventListener('mousemove', (e) => {
          if (isMove) {
            /* eslint-disable no-console */
            console.log(el.style.left)
            el.style.left = objectX - e.clientX + "px"
            el.style.top = objectY - e.clientY + "px"
            /* eslint-disable no-console */
          }
        })

        el.addEventListener('mouseup', () => {
          isMove = false
          /* eslint-disable no-console */
          console.log(isMove)
          console.log(objectX)
          console.log(objectY)
          /* eslint-disable no-console */
        })
      }
    }
  }
}
</script>

<style lang="sass">
.myButton
  width: 200px
  height: 44px
  background: #77a44b
  font-size: 14px
  color: #fff
  position: relative
  margin: 20px 0 0
</style>

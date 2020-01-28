<template>
  <div id="app" class="main">
    <div class="header">
      <div class="wrapper wrapper-header">
        <div class="header__title">Lesson 2</div>
      </div>
    </div>
    <div class="wrapper wrapper-main">
      <button
        v-for="item in buttons"
        :key="item.id"
        v-text="item.priority"
        v-drag:[buttons]="item"
        class="myButton"
        :style="{ 
          width: item.width + 'px',
          height: item.height + 'px',
          left: item.left + 'px',
          top: item.top + 'px',
          background: item.color,
          zIndex: [ item.hasParent ? 2 : 0 ]
        }"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data: () => ({
    buttons: [
      {
        priority: 25,
        left: 0,
        top: 100,
        block: false
      },
      {
        priority: 28,
        left: 100,
        top: 300,
        block: false
      },
      {
        priority: 32,
        left: 500,
        top: 300,
        block: false
      },
      {
        priority: 48,
        left: 300,
        top: 500,
        block: false
      },
      {
        priority: 77,
        left: 400,
        top: 100,
        block: false
      },
    ]
  }),

  directives: {
    drag: {
      bind: (el, bin) => {
        let isMove = false,
            shiftX,
            shiftY,
            coordX = bin.value.left,
            coordY = bin.value.top

        el.addEventListener('mousedown', (e) => {
          if (!bin.value.block) {
            isMove = true
          }
    
          shiftX = e.pageX - el.offsetLeft
          shiftY = e.pageY - el.offsetTop
          el.style.zIndex = 1
          /* eslint-disable no-console */
          // console.log(bin.value.left)
          // console.log(bin.arg[0].left)
          /* eslint-disable no-console */
        })

        window.addEventListener('mousemove', (e) => {
          if (isMove) {
            coordX = e.pageX - shiftX
            coordY = e.pageY - shiftY
            
            el.style.left = coordX + 'px'
            el.style.top = coordY + 'px'
          }
        })

        el.addEventListener('mouseup', () => {
          isMove = false
          bin.value.left = coordX
          bin.value.top = coordY
          el.style.zIndex = 0

          bin.value.width = 240
          bin.value.height = 150

          if (Number.isInteger(bin.value.hasParent)) {
            bin.arg[bin.value.hasParent].isParent = false
            bin.arg[bin.value.hasParent].color = "white"
            bin.arg[bin.value.hasParent].block = false
            bin.value.hasParent = false
          }

          bin.arg.forEach(function(em, id){
            if (em.priority !=  bin.value.priority) {
              if (
                (Math.abs(em.left - bin.value.left) - parseInt(el.style.width) / 2) <= 120
                && (Math.abs(em.top - bin.value.top) - parseInt(el.style.height) / 2) <= 75
                && !em.hasParent
                && !em.isParent
              ) {
                bin.value.left = em.left
                bin.value.top = em.top
                bin.value.width = 120
                bin.value.height = 75
                bin.value.hasParent = id
                em.isParent = true

                if (bin.value.priority < em.priority) {
                  em.color = "green"
                } else {
                  em.color = "red"
                  em.block = true
                }
              }
            }
          });
        })
      }
    }
  }
}
</script>

<style lang="sass">
.myButton
  width: 240px
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

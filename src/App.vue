<template>
  <div id="app" class="main">
    <div class="header">
      <div class="wrapper wrapper-header">
        <div class="header__title">Lesson 2</div>
      </div>
    </div>
    <div class="wrapper wrapper-main">
      <div
        v-for="item in buttons"
        :key="item.id"
        v-drag:[buttons]="item"
        class="myButton"
        :style="{ 
          width: item.width + 'px',
          height: item.height + 'px',
          left: item.left + 'px',
          top: item.top + 'px',
          background: item.color,
          zIndex: [ item.parentId ? 2 : 0 ]
        }"
      >
        <span v-text="item.priority"/>
      </div>
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
        width: 240,
        height: 150,
        left: 0,
        top: 100,
        block: false,
        parentId: false,
        childId: false,
      },
      {
        priority: 28,
        width: 240,
        height: 150,
        left: 100,
        top: 600,
        block: false,
        parentId: false,
        childId: false,
      },
      {
        priority: 32,
        width: 240,
        height: 150,
        left: 500,
        top: 400,
        block: false,
        parentId: false,
        childId: false,
      },
      {
        priority: 48,
        width: 240,
        height: 150,
        left: 600,
        top: 700,
        block: false,
        parentId: false,
        childId: false,
      },
      {
        priority: 77,
        width: 240,
        height: 150,
        left: 400,
        top: 100,
        block: false,
        parentId: false,
        childId: false,
      },
    ]
  }),
  /* eslint-disable no-console */
  directives: {
    drag: {
      bind: (el, bin) => {
        let isMove = false,
            rangeX,
            rangeY,
            coordX = bin.value.left,
            coordY = bin.value.top

        el.addEventListener('mousedown', (e) => {
          if (!bin.value.block) {
            isMove = true
          }
    
          rangeX = e.pageX - el.offsetLeft
          rangeY = e.pageY - el.offsetTop
          el.style.zIndex = 1
        })

        window.addEventListener('mousemove', (e) => {
          if (isMove) {
            coordX = e.pageX - rangeX
            coordY = e.pageY - rangeY
            
            el.style.left = coordX + 'px'
            el.style.top = coordY + 'px'

            // Перемещение вложенного элемента
            if (Number.isInteger(bin.value.childId)) {
              bin.arg[bin.value.childId].left = coordX
              bin.arg[bin.value.childId].top = coordY
            }
          }
        })

        el.addEventListener('mouseup', () => {
          isMove = false
          bin.value.left = coordX
          bin.value.top = coordY
          el.style.zIndex = 0

          bin.value.width = 240
          bin.value.height = 150

          // Сброс состояния родителя
          if (Number.isInteger(bin.value.parentId)) {
            bin.arg[bin.value.parentId].childId = false
            bin.arg[bin.value.parentId].color = "#fff"
            bin.arg[bin.value.parentId].block = false
            bin.value.parentId = false
          }

          bin.arg.forEach(function(em, id){
            if (em.priority !=  bin.value.priority) {
              if (
                (Math.abs(em.left - bin.value.left) - parseInt(el.style.width) / 2) <= parseInt(el.style.width) / 2
                && (Math.abs(em.top - bin.value.top) - parseInt(el.style.height) / 2) <= parseInt(el.style.height) / 2
                && !Number.isInteger(em.parentId)
                && !Number.isInteger(em.childId)
                && !Number.isInteger(bin.value.block)
                && !Number.isInteger(bin.value.childId)
              ) {
                bin.value.left = em.left
                bin.value.top = em.top
                bin.value.width = 120
                bin.value.height = 75
                bin.value.parentId = id
                em.childId = bin.arg.findIndex(x => x.priority === bin.value.priority)

                if (bin.value.priority < em.priority) {
                  em.color = "#68bc48"
                } else {
                  em.color = "#ed2025"
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
  display: flex
  justify-content: flex-end
  align-items: flex-end
  background: #fff
  border: 3px solid #16211E
  font-size: 24px
  font-weight: 700
  line-height: 18px
  cursor: pointer
  box-shadow: rgba(59, 60, 59, 0.7) 0px 4px 31px;
  position: absolute
  padding: 10px
  transition: box-shadow .3s ease-out
  &:hover
    box-shadow: rgba(59, 60, 59, 0.2) 0px 4px 31px;
</style>

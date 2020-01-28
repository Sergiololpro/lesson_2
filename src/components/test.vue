v-stas:[text]="myText"
  <template>
  <div>
    <div
      v-for="item in items"
      v-cloak
      :key="item.id"
      class="ra-item"
    >
      <ul>
        <li
          v-text="item.name"
          v-grissli="myTestValue * 3"
          v-stas.click="myText"
        />

        <li
          v-text="item.price + ' ' +  item.currency"
          v-stas.background="myColor1"
        />
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    myTestValue: 100,

    myColor: {
      bg: "#b1b1b1",
      fontSize: '12px',
      color: "red",
      text: 'text'
    },
    
    myColor1: {
      bg: "#000000",
      fontSize: '12px',
      color: "#ffffff",
      text: 'text'
    },

    items: [
      {
        id: 101,
        name: 'Some good apple',
        price: 200,
        currency: 'USD'
      },
      {
        id: 102,
        name: 'Old banana',
        price: 2,
        currency: 'GBR'
      }
    ]
  }),

  directives: {
    stas: {
      bind: (el, bin) => {
        // el.classList.add("blueClass")
        if (bin.modifiers.background) {
          el.style.backgroundColor = bin.value.bg;
        }
        
        el.style.color = bin.value.color
        el.style.fontSize = bin.value.fontSize

        if (bin.modifiers.click) {
          el.addEventListener('click', () => {
            alert(bin.value.text)
          })
        }
      }
    },
    
    grissli: {
      bind: (el, bin) => {
        // при создании

        // bin
        let ownBin = {
          name: 'grissli',
          value: '300', // 100 * 3
          expression: '100 * 3',
          oldValue: '100',

          arg: 'check',
          modifiers: {
            try: true,
            get: true,
            up: true,
          }
        }
      },

      inserted: (el, bin) => {
        // вызывается после вставки связанного
        // элемента внутрь элемента родителя
      },

      update: (el, bin) => {
        // вызывается после обновления VNode компонента-контейнера, 
        // но, возможно, до обновления дочерних элементов.
      },

      componentUpdated: (el, bin) => {
        //  вызывается после обновления как VNode 
        // компонента-контейнера, так и VNode его потомков.
      },

      unbind: (el, bin) => {
        //  вызывается однократно, при отвязывании 
        //  директивы от элемента.      
      },
      
      
    }
  }
}
</script>

<style scoped>
  .ra-item {
    border: 1px dotted black;
    padding: 10px;
    margin: 5px;
    max-width: 20vw;
  }

  .blueClass {
    color: blue;
  }

  [v-cloak] {
    display: none;
  }
</style>
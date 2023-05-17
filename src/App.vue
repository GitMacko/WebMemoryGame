<script>
import { ref } from 'vue'
import Card from './components/Card.vue'
import { watch } from 'vue'
import { computed } from '@vue/reactivity'
import  _  from 'lodash'

export default {
  name: 'WebMemoryGame',
  components:{
    Card
  },

  setup() {
    const TablicaKart = ref([])
    const rekaGracza = ref([])
    //const status = ref('')

    const status = computed(() =>{
      if(pozostalePary.value == 0) {
        return 'Wygrywasz!'
      } else {
        return `Pozostałe karty: ${pozostalePary.value}`
     }  } )

    const pozostalePary = computed(() => {
      const pozostaleKarty = TablicaKart.value.filter(
        karta => karta.dopasowana === false).length

        return pozostaleKarty - 1
    })


    for(let i=0; i<9; i++){
      TablicaKart.value.push({
        value: 4,
        visible: false,
        position:i,
        dopasowana: false
    })
      
  }

  const restartGry = () => {
    tasujTablice()
    TablicaKart.value = TablicaKart.value.map(
      (karta, index) => {
        return {
          ...karta,
          dopasowana: false,
          visible: false,
          position: index
        }
      }
    )
  }


  const tasujTablice = () => {
    TablicaKart.value = _.shuffle(TablicaKart.value)
  }

  const odwrocKarte = (payload) =>{
  TablicaKart.value[payload.position].visible = true
  if(rekaGracza.value[0]) {
    rekaGracza.value[1] = payload
  } else {rekaGracza.value[0] = payload}
  }

  watch(rekaGracza, currentValue => { 
    if (currentValue.length === 2){
      const kartaPierwsza = currentValue[0]
      const kartaDruga = currentValue[1]
      if(kartaPierwsza.faceValue === kartaDruga.faceValue){
        TablicaKart.value[kartaPierwsza.position].dopasowana = true
        TablicaKart.value[kartaDruga.position].dopasowana = true
      } else {
      TablicaKart.value[kartaPierwsza.position].visible = false
      TablicaKart.value[kartaDruga.position].visible = false}


      rekaGracza.value.length = 0}
    }, {deep: true})

    return {
      TablicaKart,
      odwrocKarte,
      rekaGracza,
      status,
      pozostalePary,
      tasujTablice,
      restartGry
    }
  },

}





</script>

<template>
 <h1>MemorizeIT</h1>
 <section class = "plansza">

  <Card v-for = "(karta, index) in TablicaKart" 
   :key="`card-${index}` "
   :value="karta.value" 
   @wybierz-karte="odwrocKarte" 
   :position="karta.position"
   :visible="karta.visible"
   :matched="karta.dopasowana"
   />

 </section>
 <h2>{{ status }}</h2>
<p>Pozostale karty: {{ pozostalePary }}</p>
<button @click="restartGry">Restart</button>
</template>



<style scoped>
header {
  line-height: 1.5;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }
}

.plansza{
  display:grid;
  grid-template-columns:100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
}
</style>

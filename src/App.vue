<script>
import { ref } from 'vue'
import Card from './components/Card.vue'

export default {
  name: 'WebMemoryGame',
  components:{
    Card
  },

  setup() {
    const TablicaKart = ref([])
    for(let i=0; i<4; i++){
      TablicaKart.value.push({
        value: i,
        visible: false,
        position:i
    })
      
  }
  const odwrocKarte = (payload) =>{
  TablicaKart.value[payload.position].visible = true
  }

    return {
      TablicaKart,
      odwrocKarte
    }
  },

}





</script>

<template>
 <h1>MemorizeIT</h1>
 <section class = "plansza">

  <Card v-for="(karta, index) in TablicaKart"
   :key="'card-${index}'" 
   :value="karta.value" 
   @wybierz-karte="odwrocKarte" 
   :position="karta.position"
   :visible="karta.visible"
   />

 </section>

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
  grid-template-columns:100px 100px ;
  grid-template-rows: 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
}
</style>

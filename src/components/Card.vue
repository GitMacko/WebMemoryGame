<script>
export default {
  props: {
    position:{
        type:Number,
        required:true
    },
    value: {
        type: Number,
        required: true
    },
    visible:{
        type: Boolean,
        default: false
  }
  },
  setup(props,context){



const wybierzKarte = () => 
context.emit('wybierz-karte',{
    position: props.position
})
return{
    wybierzKarte
}
  }
}
</script>


<template>
    <div class="karta" @click="wybierzKarte">
        <div v-if = "visible" class="strona-karty przod ">
            {{ value }}
        </div>
        <div v-else class="strona-karty tyl">
            Tył
        </div>
    </div>
</template>
<style>

.karta{
    border: 5px solid black;
    position:relative;
}

.strona-karty{
    width: 100%;
    height: 100%;
    position:absolute;
}



.strona-karty.przod{
    background-color: blue;
}
.strona-karty.tyl{
    background-color: red;
}

</style>
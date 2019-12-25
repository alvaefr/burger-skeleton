<template>
  <div class="ingredient">
      <h3 class="head"> {{item["ingredient_"+ lang]}} </h3>

      <img class="image" :src="require('../assets/' + item.img)" width="80" height="60"/> 

      <button id="inc" v-on:click="incrementCounter($event)"> + </button>
      {{ itemCount }}
      <button id="dec" v-on:click="decrementCounter($event)"> - </button> <br>


      <span v-if="item.gluten_free"> <img class="gluten" src="@/assets/gluten2.png" height="33"> </span>
      <span v-if="item.vegan"> <img class="vegan" src="@/assets/vegan2.png" height="30"> </span>
      <span v-if="item.milk_free"> <img class="milk" src="@/assets/milk2.png" height="30"> </span>

      <!--          GAMMAL KOD           -->
<!--    <label>-->
<!--        -->

<!--      {{item["ingredient_"+ lang]}}, {{item.selling_price}}:-, &lt;!&ndash;{{item.stock}} pcs &ndash;&gt;-->

<!--    <br>-->
<!--    <button v-on:click="incrementCounter" :disabled="item.stock === 0">+</button>-->
<!--    {{ counter }}-->
<!--    </label>-->

<!--    <button v-on:click="decrementCounter"  :disabled="counter === 0">-</button>-->

  </div>
</template>
<script>
  /* eslint-disable no-console */
export default {
  name: 'Ingredient',
  props: {
    item: Object,
    lang: String,
    itemCount: Number
  },
    data: function () {
    return {
    };

  },

  methods: {
    // incrementCounter: function () {          //////////////////////////// GAMLA KODER!
    //   this.counter += 1;
    //   // sending 'increment' message to parent component or view so that it
    //   // can catch it with v-on:increment in the component declaration
    //   console.log(this.item)
    //   this.$emit('increment');
    //   //this.$emit('counter', this.counter)
    //
    // },
    //
    // decrementCounter: function () {
    //   this.counter -= 1;
    //
    //   // sending 'increment' message to parent component or view so that it
    //   // can catch it with v-on:increment in the component declaration
    //   this.$emit('decrement');
    // },
      incrementCounter: function (ev) {
          if (ev.target.id === "inc") {
              // sending 'increment' message to parent component or view so that it
              // can catch it with v-on:increment in the component declaration
              this.$emit('increment');
          }
      },
      decrementCounter: function (ev) {
          if (ev.target.id === "dec") {
              // sending 'increment' message to parent component or view so that it
              // can catch it with v-on:increment in the component declaration
              this.$emit('decrement');
          }
      },


    resetCounter: function () {
      // this.counter = 0;
    },
     updateCounter: function () {
       console.log(this.counter)
       console.log(this.item)
     }
  }
}
</script>
<style scoped>
    
    @import url('https://fonts.googleapis.com/css?family=Dosis&display=swap');
     p {
font-family: 'Dosis', sans-serif;
}
     
    .head {
        padding-left:70px;
 padding-right:5px;

    }
    
    #inc {
        background-color: rgba(177, 160, 149, 0.65);
        border-radius: 5em;

          color: rgba(103, 87, 76, 1); 
    }
    
     #dec {
        background-color: rgba(177, 160, 149, 0.65);
        border-radius: 5em;
  
                 color: rgba(103, 87, 76, 1); 
    }
    
    .price {
        
    }
    
    .image {
        float: left;
      background-color: rgba(232, 232, 232);
        border-top: 1px solid rgba(103, 87, 76, 1);
         border-bottom: 1px solid rgba(103, 87, 76, 1);
        border-left: 1px solid rgba(103, 87, 76, 1);
        border-radius: 1.3em;
        padding-top: 34px;
         padding-bottom: 33px;
        padding-left: 4px;
        padding-right: 4px;
        margin-top: -55px;
        margin-left: -15px;
    }
    
    .gluten {
        float: bottom;
        padding-top: 1em;

   
     
    }
    
     .milk {
        float: bottom;

    }
    
     .vegan {
        float: bottom;

    }
  
</style>
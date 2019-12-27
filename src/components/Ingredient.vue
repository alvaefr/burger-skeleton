<template>
  <div class="ingredient">
     <div class="info">
      <h3 class="head"> {{item["ingredient_"+ lang]}} </h3>
            <button id="inc" v-on:click="incrementCounter($event)" :disabled="item.category === 4 && itemCount >= 1">+</button>
      {{ itemCount }}
      <button id="dec" v-on:click="decrementCounter($event)">-</button> <br>
     </div>
      
      <h3 class="price">{{item.selling_price}}:- </h3>

      <img class="image" :src="require('../assets/' + item.img)" width="60" height="60"/>



      <div class="filter">
      <span v-if="item.gluten_free"> <img class="gluten" src="@/assets/gluten2.png" height="33"> </span>
      <span v-if="item.vegan"> <img class="vegan" src="@/assets/vegan2.png" height="30"> </span>
      <span v-if="item.milk_free"> <img class="milk" src="@/assets/milk2.png" height="30"> </span>
      </div>
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
    
    .ingredient {
        display: grid;
        grid-template-columns: 1fr 1.7fr;
        grid-template-rows: 1.9fr 1fr;
        grid-column-gap: 0px;
        grid-row-gap: 0px;
    }
    
    .info {
        grid-area: 1 / 2 / 2 / 3;
    }
    .head {
        padding-bottom: 0.2em;
      
    }
    
    #inc {
   
        background-color: rgba(177, 160, 149, 0.65);
        border-radius: 5em;
        border: 2px solid rgba(177, 160, 149, 1);
        color: rgba(103, 87, 76, 1); 
    }
    
     #dec {

        background-color: rgba(177, 160, 149, 0.65);
        border-radius: 5em;
        border: 2px solid rgba(177, 160, 149, 1);
        color: rgba(103, 87, 76, 1); 
    }
    
    .price {
        grid-area: 2 / 1 / 3 / 2;
      
        
    }
    
    .image {
         grid-area: 1 / 1 / 2 / 2;


    }
    
    .filter {
   grid-area: 2 / 2 / 3 / 3; 
    }


  
</style>
<template>
  <div class="ingredient">
      <h3> {{item["ingredient_"+ lang]}} </h3>
      <img class="image" :src="require('../assets/' + item.img)" width="80" height="60"/> <br>
      <button id="inc" v-on:click="incrementCounter($event)"> + </button>
      {{ itemCount }}
      <button id="dec" v-on:click="decrementCounter($event)"> - </button>
      {{item.selling_price}}:- <br>

      <span v-if="item.gluten_free"> <img src="@/assets/gluten.png" height="20"> </span>
      <span v-if="item.vegan"> <img src="@/assets/vegan.png" height="20"> </span>
      <span v-if="item.milk_free"> <img src="@/assets/milk.png" height="19"> </span>

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
  
</style>
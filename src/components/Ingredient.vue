<template>
  <div class="ingredient">
    <label>

      {{item["ingredient_"+ lang]}}, {{item.selling_price}}:-, <!--{{item.stock}} pcs -->
    <span v-if="item.gluten_free"> G </span>
    <span v-if="item.vegan"> V </span>
    <span v-if="item.milk_free"> L </span>
    <br>
    <button v-on:click="incrementCounter" :disabled="item.stock === 0">+</button>
    {{ counter }}
    </label>

    <button v-on:click="decrementCounter"  :disabled="counter === 0">-</button>

  </div>
</template>
<script>
  /* eslint-disable no-console */
export default {
  name: 'Ingredient',
  props: {
    item: Object,
    lang: String
  },
    data: function () {
    return {
      counter: 0,
    };

  },

  methods: {
    incrementCounter: function () {
      this.counter += 1;
      // sending 'increment' message to parent component or view so that it
      // can catch it with v-on:increment in the component declaration
      console.log(this.item)
      this.$emit('increment');
      //this.$emit('counter', this.counter)

    },

    decrementCounter: function () {
      this.counter -= 1;

      // sending 'increment' message to parent component or view so that it
      // can catch it with v-on:increment in the component declaration
      this.$emit('decrement');
    },

    resetCounter: function () {
      this.counter = 0;
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
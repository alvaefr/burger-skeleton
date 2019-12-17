<template>
  <div class="ingredient">
    <label>

      {{item["ingredient_"+ lang]}}, {{item.selling_price}}:-, {{item.stock}} pcs
    <span v-if="item.gluten_free"> G </span>
    <span v-if="item.vegan"> V </span>
    <span v-if="item.milk_free"> L </span>
    <br>
    <button v-on:click="incrementCounter">+</button>
    {{ counter }}
    </label>

    <button v-on:click="decrementCounter"  :disabled="item.stock === 30">-</button>

  </div>
</template>
<script>
  /* eslint-disable no-console */
export default {
  name: 'Ingredient',
  props: {
    item: Object,
    lang: String,

  },
    data: function () {
    return {
      counter: 0
    };

  },

  methods: {
    incrementCounter: function () {
      this.item.stock -= 1;
      this.counter = 30 - this.item.stock;
      // sending 'increment' message to parent component or view so that it
      // can catch it with v-on:increment in the component declaration
      this.$emit('increment');
      this.$emit('incIng');
      //this.$emit('counter', this.counter)
    },
    decrementCounter: function () {
      this.item.stock += 1;
      this.counter = 30 - this.item.stock;

      // sending 'increment' message to parent component or view so that it
      // can catch it with v-on:increment in the component declaration
      this.$emit('decrement');
      this.$emit('decIng');
    },
    resetCounter: function () {
      this.counter = 0;
    },

    updateCounter: function () {
      this.counter = 30 - this.item.stock;
    }
  }
}
</script>
<style scoped>
  
</style>
<template>
  <div class="ingredient">
    <label>
      {{item["ingredient_"+ lang]}}, {{item.selling_price}}:-, {{item.stock}} pcs
      <button v-on:click="incrementCounter">+</button>
    </label>
    {{ counter }}
    <button v-if="counter > 0" v-on:click="decrementCounter">-</button>

  </div>
</template>
<script>
export default {
  name: 'Ingredient',
  props: {
    item: Object,
    lang: String,
    cunt:0,


  },
    data:
            function () {
    return {
      counter: 0
    };

  },

  methods: {
    incrementCounter: function () {
      this.counter += 1;
      // sending 'increment' message to parent component or view so that it
      // can catch it with v-on:increment in the component declaration
      this.$emit('increment');
      this.$emit('counter', this.counter)
    },
    decrementCounter: function () {
        this.counter -= 1;
      // sending 'increment' message to parent component or view so that it
      // can catch it with v-on:increment in the component declaration
      this.$emit('decrement');
    },
    resetCounter: function () {
      this.counter = 0;
    }
  }
}
</script>
<style scoped>
  
</style>
<template>
  <!-- Note in this component that it is using another component -->
<div>
  <OrderItem
    :ui-labels="uiLabels"
    :lang="lang"
    :order="order"
    :categoryNum ="categoryNum">
  </OrderItem>
<<<<<<< HEAD
  <input type="checkbox" id="ossm" name="ossm" v-on:click = "orderDone">
  <label for="ossm"> DONE! </label>
=======
    
    
 <input type="checkbox" id="ossm" name="ossm"> 
<label for="ossm"> DONE </label> 
>>>>>>> f6588be30fbc6d06ec28b3b043bd1fa4417acefd
  </div>
    
    
</template>
<script>
import OrderItem from '@/components/OrderItem.vue'
export default {
  name: 'OrderItemToPrepare',
  components: { OrderItem },
  props: {
    uiLabels: Object,
    order: Object,
    orderId: String,
    lang: String,
    categoryNum: Array
  },
  data: function(){
    return {  button_status: true}
  },
  methods: {
    orderDone: function () {
      // sending 'done' message to parent component or view so that it
      // can catch it with v-on:done in the component declaration
      this.$emit('done');
    },
    cancelOrder: function () {
      // not implemented
    }
  }
}
</script>
<style scoped>
  #ossm {
          display: none;
                margin: 5%;
      }

<<<<<<< HEAD
  input[type=checkbox] + label {
=======
    #ossm {
        display: none;
    }
input[type=checkbox] + label {

  color: #ccc;
    background-color: grey;
  font-style: italic;
} 
input[type=checkbox]:checked + label {
  color: #f00;
  background-color: green;
  font-style: normal;
}
>>>>>>> f6588be30fbc6d06ec28b3b043bd1fa4417acefd

    color: black;
      background-color: green;
    font-family: "Courier new", monospace;
    border: 2px solid darkgreen;
  }
</style>

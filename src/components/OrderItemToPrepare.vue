<template>
  <!-- Note in this component that it is using another component -->
<div>
  <OrderItem
    :ui-labels="uiLabels"
    :lang="lang"
    :order="order"
    :categoryNum ="categoryNum">
  </OrderItem>
    
    
 <input type="checkbox" id="ossm" name="ossm"> 
<label for="ossm"> DONE </label> 
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
.doneButton {
  background-color: #4CAF50;
border: none;
color: white;
padding: 15px 32px;
text-align: center;
text-decoration: none;
display: inline-block;
font-size: 16px;
margin: 4px 2px;
font-family: "Courier new", monospace;
}

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

</style>

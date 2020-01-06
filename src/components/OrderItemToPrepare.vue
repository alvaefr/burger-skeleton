<template>
  <!-- Note in this component that it is using another component -->
<div>
  <OrderItem
    :ui-labels="uiLabels"
    :lang="lang"
    :order="order"
    :categoryNum ="categoryNum">
  </OrderItem>

  <input type="checkbox" id="ossm" name="ossm" v-on:click = "orderDone">
  <label for="ossm"> DONE! </label>

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

input[type=checkbox] + label {

  color: #ccc;
    background-color: green;
  font-style: italic;
}
input[type=checkbox]:checked + label {
  color: #f00;
  background-color: red;
  font-style: normal;
}

</style>

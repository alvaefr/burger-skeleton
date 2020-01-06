<template>
  <!-- Note in this component that it is using another component -->
<div>
  <OrderItem
    :ui-labels="uiLabels"
    :lang="lang"
    :order="order"
    :categoryNum ="categoryNum"
    :burger="burgers"
    :sides="sides">
  </OrderItem>

  <input type="checkbox" id="ossm" name="ossm" v-on:click = "orderDone(order)">
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
    categoryNum: Array,
    burgers: '',
    sides: '',
  },
  data: function(){
    return {  button_status: true}
  },
  methods: {
    orderDone: function (order) {
      // sending 'done' message to parent component or view so that it
      // can catch it with v-on:done in the component declaration

      if (this.burgers === 1) {
        this.$emit('Bdone', order);
      }else {
        this.$emit('Sdone', order);
      }

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

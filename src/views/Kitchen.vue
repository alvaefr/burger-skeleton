<template>

  <div>
<div id="orders" class="grid-container">
  <div v-show="category_view === 'Burger'">
    <div class="item1">{{category_view}}</div>

  <div class="item2">
      <!--<h1>orders:{{ uiLabels.ordersInQueue }}</h1>-->
    <OrderItemToPrepare
      v-for="(order, key) in orders"
      v-if="order.status !== 'done' && order.ingredient === senap"
      v-on:done="markDone(key)"
      :order-id="key"
      :order="order"
      :ui-labels="uiLabels"
      :lang="lang"
      :key="key">
    </OrderItemToPrepare>
  </div>
  <button v-on:click="change_view(); setCategory_view('')">Back</button>
  </div>
  <div class="item3" v-show="category_view === 'Done orders'">
      <div class="item1">{{category_view}}</div>
      <h1>Finished: {{ uiLabels.ordersFinished }}</h1>
    <OrderItem
      v-for="(order, key) in orders"
      v-if="order.status === 'done' && order"
      :order-id="key"
      :order="order"
      :lang="lang"
      :ui-labels="uiLabels"
      :key="key">
    </OrderItem>
    <button v-on:click="setCategory_view('')">Back</button>
  </div>
</div>
<div v-show="category_view == ''">
<button v-on:click= "setCategory_view('Burger')">Burger</button>
<button v-on:click="setCategory_view('Drinks and sides')">Drinks and sides</button>
<button v-on:click="setCategory_view('Done orders')">Done orders</button>
</div>
</div>
</template>
<script>
  import OrderItem from '@/components/OrderItem.vue'
import OrderItemToPrepare from '@/components/OrderItemToPrepare.vue'

//import methods and data that are shared between ordering and kitchen views
import sharedVueStuff from '@/components/sharedVueStuff.js'

export default {
  name: 'Ordering',
  components: {
    OrderItem,
    OrderItemToPrepare
  },
  mixins: [sharedVueStuff], // include stuff that is used in both
                            //the ordering system and the kitchen
  data: function(){
    return {
      chosenIngredients: [],
      price: 0,
        burger_view: false,
        category_view: '',
        category_number: 1
    }
  },
  methods: {
    markDone: function (orderid) {
      this.$store.state.socket.emit("orderDone", orderid);
    },
    setCategory_view: function(view) {
            this.category_view = view;
    },
    change_view: function(){
      this.burger_view = !this.burger_view;
    }
  }
}
</script>
<style scoped>
	#orders {
    font-size:24pt;
  }

  h1 {
    text-transform: uppercase;
    font-size: 1.4em;
  }

  .grid-container {
      display: grid;
      grid-template-areas:'header header header header header header'
    'menu main main main right right'
    'menu footer footer footer footer footer';
 grid-gap: 10px;
 background-color: #2196F3;
 padding: 10px;
}

.grid-container > div{
 background-color: rgba(255, 255, 255, 0.8);
 border: 1px solid black;
 text-align: center;
 font-size: 30px;
  }

  .item1 {
    grid-area: header;
  }

  .item2 {
    grid-area: main;
  }

  .item3 {
    grid-area: right;
  }
</style>

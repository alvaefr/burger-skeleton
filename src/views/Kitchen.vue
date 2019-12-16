<template>
<div>
  <div v-show="category_view === 'Burger'">
    <div id="orders" class="grid-container">
    <div class="item1">{{category_view}}</div>

  <div class="item2">
      <!--<h1>orders:{{ uiLabels.ordersInQueue }}</h1>-->
    <OrderItemToPrepare
      v-for="(order, key) in orders"
      v-if="order.status !== 'done'"
      v-on:done="markDone(key)"
      :order-id="key"
      :order="order"
      :ui-labels="uiLabels"
      :lang="lang"
      :key="key">
    </OrderItemToPrepare>
  </div>
  <button id= back class = 'item3' v-on:click="change_view(); setCategory_view('')">Back</button>
  </div>
  </div>
  <div v-show="category_view === 'Done orders'" class="grid-container">
      <div class="item1">{{category_view}}</div>
      <h1>Finished: {{ uiLabels.ordersFinished }}</h1>
        <div class="item2">
    <OrderItem
      v-for="(order, key) in orders"
      v-if="order.status === 'done' && order"
      :order-id="key"
      :order="order"
      :lang="lang"
      :ui-labels="uiLabels"
      :key="key">
    </OrderItem>
      </div>
  <button id= back class = 'item3' v-on:click="change_view(); setCategory_view('')">Back</button>
  </div>
<div v-show="category_view === ''" class = grid-container_option>
  <div>
<button id = 'button' v-on:click= "setCategory_view('Burger')">Burger</button>
</div>
<div>
  <button id = 'button' v-on:click="setCategory_view('Drinks and sides')">Drinks and sides</button>
</div>
<div>
  <button id = 'button' v-on:click="setCategory_view('Done orders')">Done orders</button>
</div>

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
    'main main main main main main'
    'button_back footer footer footer footer footer';
    grid-template-rows: 10% 80% 10%;
    width: 36em;
    height: 15em;
 grid-gap: 1px;
 background-color: black;
   text-align: center;
 padding: 10px;
}

.grid-container > div{
 background-color: white;
 opacity: 0.7;
 font-size: 30px;
 margin: 0.2%;
   border-radius: 0.5em;
     font-family: "Courier new", monospace;
  }

  .grid-container_option {
      display: grid;
    grid-template-columns:33% 33% 33%;
    width: 78em;
    height: 31em;
 background-color: black;
 padding: 5px;
}

.grid-container_option > div{
  margin: 3%;
  }

#button {
  padding: 10px 24px;
  border-radius: 8px;
  background-color: white;
  color: black;
  border: 2px solid #e7e7e7;
  font-size: 50px;
  width: 80%;
  height: 80%;
margin:3% 20% 3% 5%;
}

#back {
  text-align: center;
  font-size: 0.7em;
  background-color: red;
  border-radius: 0.5em;
    font-family: "Courier new", monospace;
}
  .item1 {
    grid-area: header;
  }

  .item2 {
    grid-area: main;
  }

  .item3 {
    grid-area: button_back;
  }
</style>

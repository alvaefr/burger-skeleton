<template>
<div id="orders">

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
<!-- <div v-for ="order in orders">
  <div v-for ="burgers in order">
  <div v-for = "ingredient in burgers.ingredients">
          category_vue: {{ingredient}}
  </div>
  </div>
</div> -->
  <div v-show="category_view === 'Burger'">
  <h1>Burger</h1>
  <div v-for="(order, key) in undoListReverse" :key="key">
    status: {{order.orderId}} {{order.status}}
  <!-- <div v-show ="order.status === 'done'"> -->
  <button id= 'button' v-on:click="orderUnDone(order.orderId)">
Undo: {{order.orderId}}
  </button>
  <!-- </div> -->

    </div>
  {{$store.state.hello}}
  <div>
    <OrderItemToPrepare
      v-for="(order, key) in orders"
      v-if="order.status !== 'done' && isCategory(category_burger, order.burgers)"
      v-on:done="markDone(key)"
      :order-id="key"
      :order="order"
      :ui-labels="uiLabels"
      :lang="lang"
      :key="key"
      :categoryNum = "category_burger">
    </OrderItemToPrepare>
  <button id= back class = 'item3' v-on:click="setCategory_view('')">Back to overview</button>
  </div>
  </div>
  <div v-show="category_view === 'Drinks and sides'">
  <h1>Sides and drink</h1>
  <div>
    <OrderItemToPrepare
      v-for="(order, key) in orders"
      v-if="order.status !== 'done' && isCategory(category_drSi, order.burgers)"
      v-on:done="markDone(key)"
      :order-id="key"
      :order="order"
      :ui-labels="uiLabels"
      :lang="lang"
      :key="key"
      :categoryNum = "category_drSi">
    </OrderItemToPrepare>
      <button id= back class = 'item3' v-on:click="setCategory_view('')">Back to overview</button>
  </div>
  </div>
    <div v-show="category_view === 'Done orders'">
        <h1>{{ uiLabels.ordersFinished }}</h1>
  <div>
    <OrderItem
      v-for="(order, key) in orders"
      v-if="order.status === 'done'"
      :order-id="key"
      :order="order"
      :lang="lang"
      :ui-labels="uiLabels"
      :key="key"
      :categoryNum = "category_burger">
    </OrderItem>
        <button id= back class = 'item3' v-on:click="setCategory_view('')">Back to overview</button>
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
  data: function() {
    return {
          category_burger:[1,2,3,4],
          category_drSi:[5,6],
          category_all:[1,2,3,4,5,6],
          category_view: '',
          undoList: []
    }
  },
  computed: {undoListReverse: function () {return this.undoList.reverse()}
  //   countBeef100: function() {
  //     return this.countNumberOfIngredients(2)
  //   },
  //   countAllIngredients: function() {
  //     let ingredientTuples = []
  //     for (let i = 0; i < this.ingredients.length; i += 1) {
  //       ingredientTuples[i] = {};
  //       ingredientTuples[i].name = this.ingredients[i]['ingredient_' + this.lang];
  //       ingredientTuples[i].count = this.countNumberOfIngredients(this.ingredients[i].ingredient_id);
  //     }
  //     return ingredientTuples;
  //   }
  },
  methods: {
    isCategory: function (arr, itemArray) {

      for(let i = 0; i < arr.length; i += 1){
        for(let j = 0; j < itemArray.length; j += 1){
          for(let k = 0; k < itemArray[j].ingredients.length; k += 1){
           if (arr[i] === itemArray[j].ingredients[k].category){
              //console.log("itemArray", itemArray[j].ingredients[k].category);
              return true;
            }
          }
        }
      }
      return false;
    },
    markDone: function (orderid) {
      this.$store.state.socket.emit("orderDone", orderid);
      this.undoList.push(this.orders[orderid]);
      if (this.undoList.length >3)
        this.undoList.shift();
    },
    orderUnDone: function (orderid) {
        this.$store.state.socket.emit("orderNotStarted", orderid);
    },
    setCategory_view: function(view) {
    				this.category_view = view;
    }
    // countNumberOfIngredients: function (id) {
    //   let counter = 0;
    //   for (let order in this.orders) {
    //     //Now we have an array of burgers in an order so we need to add a loop
    //     let burgers = this.orders[order].burgers;
    //     for (let j = 0; j < burgers.length; j += 1) {
    //       for (let i = 0; i < burgers[j].ingredients.length; i += 1) {
    //         if (this.orders[order].status !== "done" &&
    //             burgers[j].ingredients[i].ingredient_id === id) {
    //           counter +=1;
    //         }
    //       }
    //     }
    //   }
    //   return counter;
    // }
  }
}
</script>
<style scoped>
	#orders {
    font-size:24pt;
    font-family: "Courier new", monospace;
  }
  h1 {
    text-transform: uppercase;
    font-size: 1.4em;
  }
  #back {
  font-family: "Courier new", monospace;
  background-color: red;
  border-radius: 0.5em;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
}
.grid-container_option {
    display: grid;
  grid-template-columns:33% 33% 33%;
  width: 1fr;
  height: 15em;
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

</style>

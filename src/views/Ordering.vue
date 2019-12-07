



<template >
<section class="example-panel">
    

  <div class="grid-container">
  
      


    <div class="Top">
      <button v-on:click="switchLang()">{{ uiLabels.language }}</button>
    </div>


  <div class="OrderList">
    <Ingredient

            ref="ingredient"
            v-for="item in ingredients"
            v-on:counter="keepCount"
            v-on:increment="addToOrder(item)"
            v-on:decrement="delFromOrder(item)"

            :item="item"
            :count="item.counter"
            :lang="lang"
            :key="item.ingredient_id"
    v-on:click="addToOrder(item)">
    </Ingredient>

  </div>



    <div class="Burger">
      <h1>{{ uiLabels.ordersInQueue }}</h1>
      <h1>{{ uiLabels.order }}</h1>
      <div v-for="countIng in countAllIngredients"
           :key="countAllIngredients.indexOf(countIng)">
        {{countIng.name}}: {{countIng.count}}
      </div>
      <div> {{ chosenIngredients.map(item => item["ingredient_"+lang]).join(', ') }}, {{ price }} kr {{this.count}} </div>
<!--      v-if="chosenIngredients.includes(this.chosenIngredients.map(item => item["ingredient_"+lang]))" Lägg till language i if-satsen-->
        <OrderItem
        v-for="(order, key) in orders"
        v-if="order.status !== 'done'"
        :order-id="key"
        :order="order" 
        :ui-labels="uiLabels"
        :lang="lang"
        :key="key">
      </OrderItem>
    </div>


    <div class="Total">
      <h2>Totalt:</h2>
      <p> {{ price }}:-</p></div>

    <div class="Done">
      <button v-on:click="placeOrder()">{{ uiLabels.placeOrder }}</button>
    </div>
  </div>
</section>
</template>

<script>

//import the components that are used in the template, the name that you
//use for importing will be used in the template above and also below in
//components
import Ingredient from '@/components/Ingredient.vue'
import OrderItem from '@/components/OrderItem.vue'

//import methods and data that are shared between ordering and kitchen views
import sharedVueStuff from '@/components/sharedVueStuff.js'

/* eslint-disable no-console */
/* instead of defining a Vue instance, export default allows the only
necessary Vue instance (found in main.js) to import your data and methods */
export default {
  name: 'Ordering',
  components: {
    Ingredient,
    OrderItem
  },
  mixins: [sharedVueStuff], // include stuff that is used in both 
                            // the ordering system and the kitchen
  data: function() { //Not that data is a function!
    return {
      chosenIngredients: [],
      price: 0,
      orderNumber: "",
      count:0
    }
  },
  created: function () {
    this.$store.state.socket.on('orderNumber', function (data) {
      this.orderNumber = data;
    }.bind(this));
  },
  computed:{
    countAllIngredients: function() {  //inkopierad från git, branch:severalburgers,kitchen
      let ingredientTuples = []
      for (let i = 0; i < this.chosenIngredients.length; i += 1) { //Skippa for-satsen?
        ingredientTuples[i] = {};
        ingredientTuples[i].name = this.chosenIngredients[i]['ingredient_' + this.lang];
        ingredientTuples[i].count = this.countNumberOfIngredients(this.chosenIngredients[i].ingredient_id);

       //console.log(this.chosenIngredients[i].ingredient_id)
      }
      //console.log(ingredientTuples)
      return ingredientTuples;

    }
  },
  methods: {
    addToOrder: function (item) {
      this.chosenIngredients.push(item);
      this.price += item.selling_price;
      //item.counter = this.count;
    },
    countNumberOfIngredients: function (id) {
      let counter = 0;

      for (let order in this.chosenIngredients) {
        //console.log(order);
        console.log(this.chosenIngredients[order])
        //let toppings = this.chosenIngredients[order]; //hittaar inte ingerdient_id utan index på chosenIngredients
        //console.log(toppings.length)
        //for (var i = 0; i < toppings.length; i += 1) ;
        //{
          if (this.chosenIngredients[order].ingredient_id === id) { //this.orders[order].status !== "done" &&
            counter += 1;
          }
       // }
      }
      return counter;
      },


    delFromOrder: function (item) {
      this.chosenIngredients.splice(this.chosenIngredients.indexOf(item),1);
      this.price -= item.selling_price;
    },
    keepCount: function(counter){
      this.count = counter;
    },

    placeOrder: function () {
      var i,
      //Wrap the order in an object
        order = {
          ingredients: this.chosenIngredients,
          price: this.price
        };
      // make use of socket.io's magic to send the stuff to the kitchen via the server (app.js)
      this.$store.state.socket.emit('order', {order: order});
      //set all counters to 0. Notice the use of $refs
      for (i = 0; i < this.$refs.ingredient.length; i += 1) {
        this.$refs.ingredient[i].resetCounter();
      }
      this.price = 0;
      this.chosenIngredients = [];
    }
  }
}
</script>
<style scoped>
/* scoped in the style tag means that these rules will only apply to elements, classes and ids in this template and no other templates. */


.example-panel {

  left:0;
  top:0;

}
.ingredient {
  border: 1px solid #ccd;
  padding: 1em;
  background-image: url('~@/assets/exampleImage.jpg');
  color: white;
}




.grid-container {
  display: grid;
  grid-template-columns: 1fr 0.8fr 1.2fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-template-areas: "Top Top Top Top Top Top Top Top Burger Burger Burger" "Top Top Top Top Top Top Top Top Burger Burger Burger" "Top Top Top Top Top Top Top Top Burger Burger Burger" "OrderList OrderList OrderList OrderList OrderList OrderList OrderList OrderList Burger Burger Burger" "OrderList OrderList OrderList OrderList OrderList OrderList OrderList OrderList Burger Burger Burger" "OrderList OrderList OrderList OrderList OrderList OrderList OrderList OrderList Burger Burger Burger" "OrderList OrderList OrderList OrderList OrderList OrderList OrderList OrderList Burger Burger Burger" "OrderList OrderList OrderList OrderList OrderList OrderList OrderList OrderList Total Total Total" "Done Done Done Done Done Done Done Done Done Done Done" "Done Done Done Done Done Done Done Done Done Done Done";
  background-color: rgba(211, 211, 211, 0.65); 
  border-radius: 7%;
  width: 80%;
  height: 80%;
  padding: 3%;
  margin: auto;
}




.Top { grid-area: Top; }

.OrderList { grid-area: OrderList;
      display: grid;

      grid-template-columns: repeat(auto-fill, 8em);
      grid-gap: 7%;
      height: 400px;
      overflow-y: scroll;}

.Done { grid-area: Done; }



.Total { grid-area: Total;
  background-color: rgba(0,0,0,0.2);
  border-radius: 1em;
}
.Total h2,
.Total p {
  margin: 0;
  display: inline-block;
  padding: 5px;
  padding-top: 1em;
  font-family: 'Open Sans', sans-serif;
  font-size: 30px;
  line-height: 28px;
}
.Total h2 {
  font-weight: bold;
  font-size: 30px;
}
.Total p{
  text-align:center;
  padding-right:2em;
  float:unset;
}

.Burger { grid-area: Burger;}








</style>
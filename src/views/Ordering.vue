<template>

<section class="example-panel">


<div class="grid-container">

<div class="Top">
<button v-on:click="switchLang()">{{ uiLabels.language }}</button>
  <div class="tab">
  <button class="tablinks" v-on:click="setCategory(1)">{{ uiLabels.puck }}</button>
  <button class="tablinks" v-on:click="setCategory(4)">{{ uiLabels.bread }}</button>
  <button class="tablinks" v-on:click="setCategory(2)">{{ uiLabels.topping }}</button>
  <button class="tablinks" v-on:click="setCategory(3)">{{ uiLabels.sauce }}</button>
  <button class="tablinks" v-on:click="setCategory(5)">{{ uiLabels.sides }}</button>
  <button class="tablinks" v-on:click="setCategory(6)">{{ uiLabels.drink }}</button>

  </div>
</div>



  <div class="OrderList">


    <Ingredient
            ref="ingredient"
            v-for="item in ingredients"
            v-if="item.category===categorynumber"
            v-on:increment="addToOrder(item)"
            v-on:decrement="delFromOrder(item)"
            :item="item"
            :count="item.counter"
            :lang="lang"
            :key="item.ingredient_id">
    </Ingredient>


  </div>



    <div class="Burger">
      <h1>{{ uiLabels.ordersInQueue }}</h1>
      <h1>{{ uiLabels.order }}</h1>
      <div v-for="countIng in countAllIngredients"
           :key="countAllIngredients.indexOf(countIng)">
             {{countIng.name}}: {{countIng.count}} <br>
      </div>
        <!-- <div>{{  countAllIngredients }}</div>-->
           <!--    <div> {{ chosenIngredients.map(item => item["ingredient_"+lang]).join(', ') }}, {{ price }} kr {{this.count}} </div>
           v-if="chosenIngredients.includes(this.chosenIngredients.map(item => item["ingredient_"+lang]))" Lägg till language i if-satsen-->


    </div>


    <div class="Total">
      <h2>{{ uiLabels.total }}:</h2>
      <p> {{ price }}:-</p></div>

    <div class="Done">
      <button id=PlaceOrderButton v-on:click="placeOrder()">{{ uiLabels.placeOrder }}</button>

      <button id=glutenButton v-on:click="showGlutenFree()" >{{ uiLabels.glutenFilter }}</button>

      <button id="nextPage" v-on:click="doneBurger()"><a href="./#/overview">page 2</a> ?</button>>

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
      glutenFilter: false,
      count:0,
      brodcategory: false,
      categorynumber: 1


    }
  },
  created: function () {
    this.$store.state.socket.on('orderNumber', function (data) {
      this.orderNumber = data;
    }.bind(this));
    this.$store.state.socket.on('')
  },
  computed:{
    countAllIngredients: function() {  //inkopierad från git, branch:severalburgers,kitchen
        let ingredientTuples = []
      for (let i = 0; i < this.chosenIngredients.length; i += 1) {
        ingredientTuples[i] = {};
        ingredientTuples[i].name = this.chosenIngredients[i]['ingredient_' + this.lang];
        ingredientTuples[i].count = this.countNumberOfIngredients(this.chosenIngredients[i].ingredient_id);
      }
      var difIngredients = Array.from(new Set(ingredientTuples.map(o => o.name)))
              .map(name => {
                return {
                  name: name,
                  count: ingredientTuples.find(o => o.name === name).count
                };
              });
      console.log(difIngredients)
      return difIngredients;
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
        //let toppings = this.chosenIngredients[order];
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


    setCategory: function(number) {
            this.categorynumber = number;
    },


    delFromOrder: function (item) {
      this.chosenIngredients.splice(this.chosenIngredients.indexOf(item),1);
      this.price -= item.selling_price;
    },

    doneBurger: function() {
      var burger = {
          ingredients: this.chosenIngredients,
          price: this.price
              };
     // this.$store.state.socket.emit('burger', {burger: burger});
      this.chosenIngredients = [];
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
    },


      showGlutenFree: function(){
          this.glutenFilter = !this.glutenFilter
          //sätt så bara glutenfria alternativ visas
      }
  }
}
</script>
<style scoped>
/* scoped in the style tag means that these rules will only apply to elements, classes and ids in this template and no other templates. */

    section {
    font-family: "Courier new", monospace;

    color: dimgrey;
    font-variant: inherit;
    }


.example-panel {

  left:0;
  top:0;

}
.ingredient {
  border: 1px solid grey;
  border-radius: 1.4em;
  padding: 1em;
  font-size:1.7vh;
  background-color: #bccfbc;
  color: dimgray;
}


.grid-container {
  display: grid;
  grid-template-columns: 1fr 0.8fr 1.2fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-template-areas: "Top Top Top Top Top Top Top Top Burger Burger Burger" "Top Top Top Top Top Top Top Top Burger Burger Burger" "Top Top Top Top Top Top Top Top Burger Burger Burger" "OrderList OrderList OrderList OrderList OrderList OrderList OrderList OrderList Burger Burger Burger" "OrderList OrderList OrderList OrderList OrderList OrderList OrderList OrderList Burger Burger Burger" "OrderList OrderList OrderList OrderList OrderList OrderList OrderList OrderList Burger Burger Burger" "OrderList OrderList OrderList OrderList OrderList OrderList OrderList OrderList Burger Burger Burger" "OrderList OrderList OrderList OrderList OrderList OrderList OrderList OrderList Total Total Total" "Done Done Done Done Done Done Done Done Done Done Done" "Done Done Done Done Done Done Done Done Done Done Done";

  background-image: url("https://cdn2.cdnme.se/3330886/8-3/skarmavbild_2019-12-06_kl_225839_5deacf59e087c37d7abbdea3.png");
  border-radius: 4em;
  border: 1px solid #FFF;
  width: 80%;
  height: 80%;
  padding: 3%;
  margin: auto;
}




.Top { grid-area: Top; }

.OrderList { grid-area: OrderList;
      display: grid;
      background-color: rgba(232, 232, 232, 0.92);
      padding: 5% 0% 4% 4%;
      margin-right: 15%;
      grid-template-columns: repeat(auto-fill, 8em);
      grid-template-rows: repeat(auto-fill, 4em);
      grid-gap: 7%;
      height: 400px;
      overflow-y: scroll;
      border-left: 3px solid #FFF;
      border-right: 3px solid #FFF;
      border-bottom: 3px solid #FFF;
      border-radius: 0em 0em 3em 3em;
      margin-top: -16%;


    }

.Done {
    grid-area: Done;
}



.Total {
    grid-area: Total;
    background-color: rgba(232, 232, 232, 0.92);
    border-radius: 1em;
    border-bottom: 3px solid #FFF;
    border-right: 3px solid #FFF;
    border-left: 3px solid #FFF;
    border-top: 3px dotted #FFF;
    border-radius: 0em 0em 2em 2em;
    margin-left: -2em;
}



.Total h2 {
  margin: 0;
  padding: 0.5em;
  padding-top: 1em;
  line-height: 0.5em;
  display: inline-block;
  text-align: right;
  font-weight: bold;
  font-size: 2em;
}

.Total p {
  margin: 0;
  padding: 0.5em;
  padding-top: 1em;
  line-height: 0.5em;
  display: inline-block;
  text-align: center;
  padding-right: 2em;
  float: unset;
}
/*

    border-bottom: 3px solid #FFF;  Vi synkar Total-fönstret med resten på söndag! Detta ska vara ^
    border-right: 3px solid #FFF;
    border-left: 3px solid #FFF;
    border-top: 3px dotted #FFF;
    border-radius: 0em 0em 2em 2em;
    margin-left: -2em;

  }*/


.Burger {
    padding: 1em;
    margin-left: -2em;
    grid-area: Burger;
    border-top: 3px solid #FFFFFF;
    border-right: 3px solid #FFF;
    border-left: 3px solid #FFF;
    border-radius: 2em 2em 0 0;
    background-color: rgba(232, 232, 232, 0.92);
}


    #glutenButton {
        border-radius: 50%;
        height: 50px;
        width: 50px}

    #PlaceOrderButton{
        border-radius: 50%;
        height: 50px;
        width: 50px
    }


/* Style the tab */
.tab {
  margin: 2% 15% 0% 0%;
  border-bottom: 3px solid #FFFFFF;




}

/* Style the buttons that are used to open the tab content */
.tab button {

  background-color: rgba(232, 232, 232, 0.92);
  width: 16.66667%;

 font-size: 90%;
  float: auto;
  border: none;
  outline: none;
  cursor: pointer;
  padding: 14px 16px;
  transition: 0.3s;
  border-radius: 1.5em 1.5em 0em 0em;
  border: 3px solid #FFF;
  border-bottom: 3px solid #FFFFFF;

}

/* Change background color of buttons on hover */
.tab button:hover {
 background-color: whitesmoke;
  border: 3px solid #FFF;

}
.tab button:focus{
    background-color: whitesmoke;
    border: 3px solid #FFF;
}

/* Create an active/current tablink class */
.tab button.active {
  background-color: #ddd;

}

/* Style the tab content */
.tabcontent {
  display: none;
  padding: 6px 12px;
  border: 3em solid #ddd;
  border-top: none;
  background-color: #ddd;

}

    .tablinks {
    font-family: "Courier new", monospace;
    }


</style>
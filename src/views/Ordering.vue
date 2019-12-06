



<template>
<section class="example-panel">
    

  <div class="grid-container">
  
      


    <div class="Top">
      <button v-on:click="switchLang()">{{ uiLabels.language }}</button>
        <div class="tab">
  <button class="tablinks" v-on:click="setcategory(1)">Puck</button>
  <button class="tablinks" v-on:click="setcategory(4)">Bröd</button>
  <button class="tablinks" v-on:click="setcategory(2)">Pålägg</button>
  <button class="tablinks" v-on:click="setcategory(3)">Sås</button>
  <button class="tablinks" v-on:click="setcategory(5)">Tillbehör</button>   
        <button class="tablinks" v-on:click="setcategory(6)">Dryck</button>   
 
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
            :lang="lang"
            :key="item.ingredient_id"
    v-on:click="addToOrder(item)">
        
        
    </Ingredient>
      
    

  </div>



    <div class="Burger">
      <h1>{{ uiLabels.ordersInQueue }}</h1>
      <h1>{{ uiLabels.order }}</h1>
      {{ chosenIngredients.map(item => item["ingredient_"+lang]).join(', ') }}, {{ price }} kr
      <OrderItem
        v-for="(order, key) in orders"
        v-if="order.status !== 'done'"
        :order-id="key"
        :order="order" 
        :ui-labels="uiLabels"
        :lang="lang"
        :key="key">
      </OrderItem>
      <footer id="BurgerFooter"> Totalt</footer>
    </div>

    <div class="Total">Totalt</div>

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
     brodcategory: false,
    categorynumber: 1
    }
  },
  created: function () {
    this.$store.state.socket.on('orderNumber', function (data) {
      this.orderNumber = data;
    }.bind(this));
  },
  methods: {
    addToOrder: function (item) {
      this.chosenIngredients.push(item);
      this.price += item.selling_price;
    },
      
    setcategory: function(number) {
            this.categorynumber = number;
        
        
    },

    delFromOrder: function(item) {
      this.chosenIngredients.pop(item);
      this.price -= item.selling_price;
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
  }

.Burger { grid-area: Burger;}


/* Style the tab */
.tab {
  overflow: hidden;
  border: 1px solid #ccc;
  background-color: #f1f1f1;
  border-radius: 5%;
}

/* Style the buttons that are used to open the tab content */
.tab button {
  background-color: inherit;
  float: left;
  border: none;
  outline: none;
  cursor: pointer;
  padding: 14px 16px;
  transition: 0.3s;
  border-radius: 5%;
}

/* Change background color of buttons on hover */
.tab button:hover {
  background-color: burlywood;
}
.tab button:focus{
    background-color:#ddd;
}

/* Create an active/current tablink class */
.tab button.active {
  background-color: #ddd;
}

/* Style the tab content */
.tabcontent {
  display: none;
  padding: 6px 12px;
  border: 1px solid #ddd;
  border-top: none;
  background-color: #ddd;
}
    
   





</style>
  <template>
  <div id="orders">

  <!-- Overview  -->
    <div v-show="category_view === ''" class = 'grid-container_option'>
      <div>
    <button class = 'startButton' id = 'button' v-on:click= "setCategory_view('Burger')">Burger</button>
    </div>
    <div>
      <button class = 'startButton' id = 'button' v-on:click="setCategory_view('Drinks and sides')">Drinks and sides</button>
    </div>
    <div>
      <button class = 'startButton' id = 'button' v-on:click="setCategory_view('Done orders')">Done orders</button>
    </div>
    </div>
  <!-- Overview End -->

  <!-- <div v-for ="order in orders">
    <div v-for ="burgers in order">
    <div v-for = "ingredient in burgers.ingredients">
            category_vue: {{ingredient}}
    </div>
    </div>
  </div> -->

  <!-- Burgers overview  -->
  <div class = 'grid-container' v-show="category_view === 'Burger'">

  <div class = 'head'> Burger</div>

<!-- Undo Button -->
    <div v-if = 'undoList.length >= 1'>
        <button class = 'undo' id= 'button' v-on:click="orderUnDone(undoList[undoList.length-1].orderId)">
          Undo: {{undoList[undoList.length-1].orderId}}
        </button>
  </div>
<!-- <div class = 'undo'>
  <div v-for="(order, key) in undoList" :key="key">
    <div v-show ="unDoButton === true">
      <button id= 'button' v-on:click="orderUnDone(order.orderId)">
        Undo: {{order.orderId}}
      </button>
    </div>
    </div>
  </div> -->

  <div class = 'items'>
    <div class = 'grid-container-burgers'>
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
  </div> <!-- grid-container-burgers -->
</div> <!-- items -->

<button class = 'backButton' id= back v-on:click="setCategory_view('')">Back to overview</button>
</div> <!-- Burger Overview End -->

<!-- Sides and Drinks Overview -->
<div class = 'grid-container' v-show="category_view === 'Drinks and sides'">

  <div class = 'head'>Sides and drinks</div>

  <!-- Undo Button -->
  <div v-if = 'undoList.length >= 1'>
      <button class = 'undo' id= 'button' v-on:click="orderUnDone(undoList[undoList.length-1].orderId)">
        Undo: {{undoList[undoList.length-1].orderId}}
      </button>
</div>
    <!-- <div v-for="(order, key) in undoList" :key="key">
      <div v-show ="unDoButton === true">
        <button id= 'button' v-on:click="orderUnDone(order.orderId)">
          Undo: {{order.orderId}}
        </button>
      </div>
    </div> -->

    <div class = 'items'>
      <div class = 'grid-container-burgers'>
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
      </div>
    </div>

  <button class = 'backButton' id= back v-on:click="setCategory_view('')">Back to overview</button>

</div><!-- Sides and Drinks Overview End-->

<!-- Finished orders-->
<div class = 'grid-container' v-show="category_view === 'Done orders'">

  <div class = 'head'>Finished orders</div>

  <div class = 'items'>
    <div class = 'grid-container-burgers'>
      <OrderItem
      v-for="(order, key) in orders"
      v-if="order.status === 'done'"
      :order-id="key"
      :order="order"
      :lang="lang"
      :ui-labels="uiLabels"
      :key="key"
      :categoryNum = "category_all">
    </OrderItem>
    </div>
  </div>
  <button id= back class = 'backButton' v-on:click="setCategory_view('')">Back to overview</button>
</div> <!-- Finished orders End-->

</div> <!-- Orders End -->

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
            undoList: [],
            unDoButton: true,
            doneOrder: 0,
                }
    },
    computed:{
      // let newUndoList = {};
      // for (let i = 0; i < this.undoList; i +=1){
      //   for(let j = 0; i <this.undoList; j +=1){
      //   if (!newUndolist.includes(this.undoList[j]))
      //       newUndoList[i] = this.undoList[j];
      //     }
      //     }
      // }
      // return [...new Set(this.undoList)]}
      // undoListReverse: function () {return this.undoList.reverse()}
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
                return true;
              }
            }
          }
        }
        return false;
      },
      markDone: function (orderid) {
        this.$store.state.socket.emit("orderDone", orderid);
        if (this.exsitsinList(orderid) === true){
          this.undoList.push(this.orders[orderid])}
        this.unDoButton = true;
        // if (this.undoList.length >3){
      },
      exsitsinList: function(orderid){
        if (this.undoList.length >= 1){
        for (let i = 0; i < this.undoList.length; i +=1){
          let listId = this.undoList[i].orderId;
          if (listId == orderid){
          return false;
        }
      }
    }
      return true;
    },
      orderUnDone: function (orderid) {
          this.$store.state.socket.emit("orderNotStarted", orderid);
          this.unDoButton = false;
          this.undoList.pop();
      },
      setCategory_view: function(view) {
      				this.category_view = view;
      },
      ingredientsCat: function(order){
        let burgers = order.burgers;
      	for (let j = 0; j < burgers.length; j += 1) {
      		for (let i = 0; i < burgers[j].ingredients.length; i += 1) {
            this.ingredientsCat.push(burgers[j].ingredients[i].category)
      }
    }
},


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

  .grid-container_option {
      display: grid;
    grid-template-columns:33% 33% 33%;
    width: 1fr;
    height: 15em;
  background-color: white;
  padding: 5px;
  }

  .grid-container_option > div{
  margin: 3%;
  }

.startButton {
    padding: 5%;
    border-radius: 3%;
    background-color: cornflowerblue;
    color: black;
    border: 2px solid darkblue;
    font-size: 100%;
    width: 80%;
    height: 80%;
  margin:3% 20% 3% 5%;
       text-align: center;
  }
.startButton:hover {
      background-color: darkblue;
  }

    .grid-container {
        display: grid;
        grid-template-areas:"back header header header header undo"
      "main main main main main main"
      "main main main main main main";
      grid-template-rows: 15% 75% 10%;
      width: 1fr;
      height: 15em;

   background-color: white;
     text-align: center;
   padding: 10px;
  }

  .grid-container > div {
   background-color: lightgrey;
   margin: 0.2%;
   font-size: 30px;
     border-radius: 0.5em;
       font-family: "Courier new", monospace;
    }
  .backButton {
      grid-area:back;
      font-family: "Courier new", monospace;
      background-color: cornflowerblue;
      border-radius: 0.5em;
      border: none;
      color: white;
      padding: 15px 32px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 16px;
      margin: 1%;
    }
    .backButton:hover {
          background-color: darkblue;
      }

  .items{
    grid-area:main;
  }

  .head{
    grid-area:header;
    width: 100%;
    height: 78%;
      text-align: center;
      padding-top: 0.45em;
             text-transform: uppercase;

  }

  .undo{
    grid-area:undo;
    grid-area:back;
    font-family: "Courier new", monospace;
    background-color: red;
    border-radius: 0.5em;
    border: none;
    color: white;
    padding: 5%;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 50%;
    height: 100%;
    width: 100%;
  }

  .grid-container-burgers {
    display: grid;
          grid-template-columns: repeat(auto-fill, 30%);
  /* grid-template-rows:33% 33% 33%; */
    background-color: lightgrey;
    padding: 2%;
    grid-gap: 3%;
    height: 12em;
          overflow-y: scroll;
             border-radius: 0.5em;

  }
  </style>

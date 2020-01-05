<template>
    <div id="orders">

        <!-- Overview  -->
        <div v-show="category_view === ''" class='grid-container_option'>
            <div>
                <button class='startButton' v-on:click="setCategory_view('Burger')">Burger</button>
            </div>
            <div>
                <button class='startButton' v-on:click="setCategory_view('Drinks and sides')">Drinks and
                    sides
                </button>
            </div>
            <div>
                <button class='startButton' v-on:click="setCategory_view('Done orders')">Done orders
                </button>
            </div>
            <div>
                <button class='startButton' v-on:click="setCategory_view('Add Ingredients')">Add
                    ingredients
                </button>
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
        <div class='grid-container' v-show="category_view === 'Burger'">

            <div class='head'> Burger</div>

            <!-- Undo Button -->
            <div v-if='undoList.length >= 1'>
                <button class='undo' v-on:click="orderUnDone(undoList[undoList.length-1].orderId)">
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

            <div class='items'>
                <div class='grid-container-burgers'>
                    <OrderItemToPrepare
                            v-for="(order, key) in orders"
                            v-if="order.status !== 'done' && isCategory(category_burger, order.burgers)"
                            v-on:done="markDone(key)"
                            :order-id="key"
                            :order="order"
                            :ui-labels="uiLabels"
                            :lang="lang"
                            :key="key"
                            :categoryNum="category_burger">
                    </OrderItemToPrepare>
                </div> <!-- grid-container-burgers -->
            </div> <!-- items -->

            <button class='backButton' v-on:click="setCategory_view('')">Back to overview</button>
        </div> <!-- Burger Overview End -->

        <!-- Sides and Drinks Overview -->
        <div class='grid-container' v-show="category_view === 'Drinks and sides'">

            <div class='head'>Sides and drinks</div>

            <!-- Undo Button -->
            <div v-if='undoList.length >= 1'>
                <button class='undo' id='button' v-on:click="orderUnDone(undoList[undoList.length-1].orderId)">
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

            <div class='items'>
                <div class='grid-container-burgers'>
                    <OrderItemToPrepare
                            v-for="(order, key) in orders"
                            v-if="order.status !== 'done' && isCategory(category_drSi, order.burgers)"
                            v-on:done="markDone(key)"
                            :order-id="key"
                            :order="order"
                            :ui-labels="uiLabels"
                            :lang="lang"
                            :key="key"
                            :categoryNum="category_drSi">
                    </OrderItemToPrepare>
                </div>
            </div>

            <button class='backButton' v-on:click="setCategory_view('')">Back to overview</button>

        </div><!-- Sides and Drinks Overview End-->

        <!-- Finished orders-->
        <div class='grid-container' v-show="category_view === 'Done orders'">

            <div class='head'>Finished orders</div>

            <div class='items'>
                <div class='grid-container-burgers'>
                    <OrderItem
                            v-for="(order, key) in orders"
                            v-if="order.status === 'done'"
                            :order-id="key"
                            :order="order"
                            :lang="lang"
                            :ui-labels="uiLabels"
                            :key="key"
                            :categoryNum="category_all">
                    </OrderItem>
                </div>
            </div>
            <button id=back class='backButton' v-on:click="setCategory_view('')">Back to overview</button>
        </div> <!-- Finished orders End-->

        <div class='grid-container' v-show="category_view === 'Add Ingredients'">
            <div class='head'>Ingredients</div>
            <button class='backButton' v-on:click="setCategory_view('')">Back to overview</button>
            <button v-if="!addingIngredient" id=addIng class='addIngredients' v-on:click="startAdding()">Add ingredients</button>
            <button v-if="addingIngredient" class="addIngredients" v-on:click="addIngredient()">Add ingredient</button>

            <div class="grid-container-showIngredients" v-if="!addingIngredient">
                <ul v-for="(item,key) in ingredients"
                    :key="key">
                    {{item.ingredient_id}}: {{item["ingredient_" + lang]}} -> {{item.stock}}
                    <input type="number" v-model="item.stock">
                </ul>
            </div>
            <div v-if="this.addingIngredient">

            <div class="grid-container-addIngredients">

                <span>{{uiLabels.svIngName}}</span>
                <input type="text" v-model="newIngredient.ingredient_sv" :placeholder="uiLabels.svIngName">

                <span>{{uiLabels.enIngName}}</span>
                <input type="text" v-model="newIngredient.ingredient_en" :placeholder="uiLabels.enIngName">

                <span>Category</span>
                <select v-model.number="newIngredient.category">
                    <option value="1">{{uiLabels.puck}}</option>
                    <option value="2">{{uiLabels.topping}}</option>
                    <option value="3">{{uiLabels.sauce}}</option>
                    <option value="4">{{uiLabels.bread}}</option>
                    <option value="5">{{uiLabels.sides}}</option>
                    <option value="6">{{uiLabels.drinks}}</option>
                </select>

                <span>{{uiLabels.lactoseFilter}}</span>
                <select v-model.number="newIngredient.milk_free">
                    <option value="1">{{uiLabels.lactoseFilter}}</option>
                    <option value="0">{{uiLabels.lactose}}</option>
                </select>

                <span>{{uiLabels.glutenFilter}}</span>
                <select v-model.number="newIngredient.gluten_free">
                    <option value="1">{{uiLabels.glutenFilter}}</option>
                    <option value="0">{{uiLabels.gluten}}</option>
                </select>

                <span>{{uiLabels.veganFilter}}</span>
                <select v-model.number="newIngredient.vegan">
                    <option value="1">{{uiLabels.veganFilter}}</option>
                    <option value="0">{{uiLabels.notVegan}}</option>
                </select>

                <span>{{uiLabels.price}}</span>
                <input type="number" v-model.number="newIngredient.selling_price" :placeholder="uiLabels.price">

                <span>{{uiLabels.stock}}</span>
                <input type="number" v-model.number="newIngredient.stock" :placeholder="uiLabels.stock">
            </div>
            </div>
        </div>

    </div> <!-- Orders End -->


</template>
<script>
    import OrderItem from '@/components/OrderItem.vue'
    import OrderItemToPrepare from '@/components/OrderItemToPrepare.vue'
    //import methods and data that are shared between ordering and kitchen views
    /* eslint-disable no-console */
    import sharedVueStuff from '@/components/sharedVueStuff.js'

    export default {
        name: 'Ordering',
        components: {
            OrderItem,
            OrderItemToPrepare
        },
        mixins: [sharedVueStuff], // include stuff that is used in both
                                  //the ordering system and the kitchen
        data: function () {
            return {
                category_burger: [1, 2, 3, 4],
                category_drSi: [5, 6],
                category_all: [1, 2, 3, 4, 5, 6],
                category_view: '',
                change: 0,
                undoList: [],
                unDoButton: true,
                addingIngredient: false,
                newIngredient: {
                    ingredient_id: 1,
                    ingredient_sv: "",
                    ingredient_en: "",
                    category: 1,
                    milk_free: 0,
                    gluten_free: 0,
                    vegan: 0,
                    selling_price: 0,
                    stock: 0,
                },
            }
        },
        computed: {
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

                for (let i = 0; i < arr.length; i += 1) {
                    for (let j = 0; j < itemArray.length; j += 1) {
                        for (let k = 0; k < itemArray[j].ingredients.length; k += 1) {
                            if (arr[i] === itemArray[j].ingredients[k].category) {
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
                if (this.exsitsinList(orderid) === true) {
                    console.log("pusha!")
                    this.undoList.push(this.orders[orderid])
                }
                this.unDoButton = true;
                // if (this.undoList.length >3){
            },
            exsitsinList: function (orderid) {
                if (this.undoList.length >= 1) {
                    for (let i = 0; i < this.undoList.length; i += 1) {
                        console.log("this", this.undoList[i].orderId);
                        let listId = this.undoList[i].orderId;
                        console.log("orderid", orderid);
                        if (listId == orderid) {
                            console.log("falskt!")
                            return false;
                        }
                    }
                }
                return true;
            },
            orderUnDone: function (orderid) {
                this.$store.state.socket.emit("orderNotStarted", orderid);
                this.unDoButton = false;
                console.log("before", this.undoList);
                this.undoList.pop();
                console.log("after", this.undoList);
            },
            setCategory_view: function (view) {
                this.category_view = view;
                this.addingIngredient = false;
            },
            startAdding: function(){
                this.addingIngredient = true;
            },
            addIngredient: function(){
                console.log(this.newIngredient)
                this.newIngredient.ingredient_id = this.ingredients.length + 1;
                this.$store.state.socket.emit("addIngredient", this.newIngredient);
                this.addingIngredient = false;
                this.newIngredient = {
                    ingredient_id: 1,
                    ingredient_sv: "",
                    ingredient_en: "",
                    category: 1,
                    milk_free: 0,
                    gluten_free: 0,
                    vegan: 0,
                    selling_price: 0,
                    stock: 0,
                };
            },
            updateStock: function (item) {
                this.$store.state.socket.emit("updateStock", {ingredient: item}, this.change);
                this.change = 0;
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
</script>
<style scoped>
    #orders {
        font-size: 24pt;
        font-family: "Courier new", monospace;
    }

    .grid-container_option {
        display: grid;
        grid-template-columns: 33% 33% 33%;
        width: 1fr;
        height: 15em;
        background-color: white;
        padding: 5px;
    }

    .grid-container_option > div {
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
        margin: 3% 20% 3% 5%;
        text-align: center;
    }

    .startButton:hover {
        background-color: darkblue;
    }

    .grid-container {
        display: grid;
        grid-template-areas: "back header header header header undo" "main main main main main main" "main main main main main main";
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
        grid-area: back;
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

    .addIngredients {
        grid-area: undo;
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

    .grid-container-showIngredients {
        grid-area: main;
        grid-template-columns: 33% 33% 33% 33%;
        font-size: 10px;
        overflow: scroll;

    }
    .grid-container-showIngredients ul {
        font-size: 10px;

    }


    .backButton:hover {
        background-color: darkblue;
    }

    .grid-container-addIngredients {
        display: grid;
        grid-area: main;
        font-size: 23px;
        grid-template-areas: "swedish" "english" "puck" "lactose" "gluten" "vegan" "price" "stock";
        grid-template-rows: 10.6% 10.6% 10.6% 10.6% 10.6% 10.6% 10.6% 10.6%;
        grid-template-columns: 50% 50%;
        width: 1fr;
        height: 15em;
        text-align: center;
        padding: 10px;
    }

    .grid-container-addIngredients input{


    }
    .grid-container-addIngredients select{
         background-color: antiquewhite;


     }
    .grid-container-addIngredients select:after{

        content: "";
        top: 14px;
        right: 10px;
        width: 0;
        height: 0;
        border: 6px solid transparent;
        border-color: #fff transparent transparent transparent;

    }


    .items {
        grid-area: main;
    }

    .head {
        grid-area: header;
        width: 100%;
        height: 78%;
        text-align: center;
        padding-top: 0.45em;
        text-transform: uppercase;

    }

    .undo {
        grid-area: undo;
        grid-area: back;
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

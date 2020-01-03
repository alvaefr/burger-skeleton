<template>
    <section class="example-panel">
        
<!--
    <div v-if="loading">
        
           <span class="popup" align=center> <h4 class=popUpText> Välkommen till burgarsidan, <br> gör en burgare i taget </h4> <img align=center src="@/assets/load-icon-png-27.png" height="30"> </span>
        
    </div>
-->


        <!--        Välkomstsida  div -->
        <div v-show="showFront === this.view" class="grid-containerFront"  >
        

            <div class="welcome">
                <!--    <img class="logo" src="/.jp" alt="BB">-->
                <img class="logo" src="@/assets/circle-cropped.png" >
                <div class=headerText>
                {{ uiLabels.welcome }}
                </div>
            </div>

            <div class="mealLocation">
                <p>{{ uiLabels.beginOrder }}</p><br>
                <button class="mealButton" v-on:click="setView(showOverview); popUp()">{{ uiLabels.eathere }}</button>
                <button class="mealButton" v-on:click="setView(showOverview), takeAway(); popUp()">{{ uiLabels.togo }}</button>
            </div>

            <div class="switchLang">
                <button class="flagButton" v-on:click="switchLang(); switchFlag()">
                    <img class="flag" v-if="picBool" src="@/assets/unionJack.jpg">
                    <img class="flag" v-else src="@/assets/sweFlag.png" > </button>
            </div>
        </div>

<!--        Ordersida div -->
        <div v-show="showMenu === this.view" class="grid-container">
            
      
            <div class="Top">
             <img id="cancelOrder" v-on:click="cancelOrder(); setView(showFront)" src="Delete-Button.png" width="65">
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
               <!-- Välj din patty <br> -->
                <div class="menuItems">

                    
                <Ingredient
                        ref="ingredient"
                        v-for="item in ingredients"
                        v-if="item.category===categoryNumber && (item.gluten_free===gluten || item.gluten_free===1) && (item.milk_free===milk || item.milk_free===1) && (item.vegan===vegan || item.vegan===1) "
                        v-on:increment="addToBurger(item); checkBurger()"
                        v-on:decrement="delFromBurger(item); checkBurger()"
                        v-bind:item="item"
                        v-bind:itemCount="ingredientCount(item)"
                        :lang="lang"
                        :key="item.ingredient_id">

                </Ingredient>

                </div>
            </div>
              

        <!-- Här visas sidomenyn med de färdiga burgarna --->
       
            <div class="Burger">
             
<!--                <h1>{{ uiLabels.ordersInQueue }}</h1>-->
<!--                <h1>{{ uiLabels.order }}</h1>-->
<!--                <div v-for="countIng in countAllIngredients"-->         
<!--                     :key="countAllIngredients.indexOf(countIng)">-->
<!--                    {{countIng.count}}-->
<!--                    {{countIng.name}}  {{countIng.ingPrice*countIng.count}} kr-->
<!--                    &lt;!&ndash;<button v-on:click="delFromBurger(countIng)"> - </button> <br> &ndash;&gt;-->
<!--                    &lt;!&ndash; button that deletes ingredient, måste kopplas till ingredient counter också &ndash;&gt;-->
<!--                </div>-->
                <h1>{{ uiLabels.order }}</h1>
                <!-- BURGARNA -->
                <hr>
                <div v-for="(item, key2) in groupIngredients(chosenIngredients)" :key="key2">
                    {{item.count}} x {{ item.ing['ingredient_' + lang] }}
                    <button v-on:click="delFromBurger(item.ing); checkBurger()"> - </button> <button v-on:click="addToBurger(item.ing)" :disabled="item.category_num === 4"> + </button>
                </div>
            </div>

            <div class="Total">
                <h2>{{ uiLabels.price }}:</h2>
                <p> {{ price }}:-</p></div>
            
            
       

            <div class="Done">

           
                
            <div class="filtergrid">
               <div class="positionGluten">
                    <label class="label">
                    <input  class="label__checkbox" type="checkbox" v-model="gluten" v-on:change="showGlutenFree()"/>
                        <span class="label__text" >
                        <span class="label__check">
                        <p align=center  >{{uiLabels.glutenFilter}}</p>
                        </span>
                        </span>
                    </label>
                </div>

                <div class="positionVegan">
                    <label class="label">
                    <input  class="label__checkbox" type="checkbox"  v-model="vegan" v-on:change="showVeganFree()"/>
                        <span class="label__text" >
                        <span class="label__check">
                        <p align=center >{{uiLabels.veganFilter}}</p>
                        </span>
                        </span>
                    </label>
                </div>

                <div class="positionMilk">
                <label class="label">
                    <input  class="label__checkbox" type="checkbox"  v-model="milk" v-on:change="showMilkFree()"/>
                        <span class="label__text" >
                        <span class="label__check">
                        <p align=center >{{uiLabels.lactoseFilter}}</p>
                        </span>
                        </span>
                </label>
                </div>
               </div>

                 

                <!-- <button v-on:click="addToOrder()"> Add to order {{ uiLabels.addToOrder }}</button>-->

                <!--<button v-on:click="setView(showFront)">{{uiLabels.backfirstpage}}</button>-->

                
                
           </div>
            

                 <button class="nextPage"  v-on:click="addToOrder()" :disabled="buttonClickable===false"> {{uiLabels.yourOrder}}</button>

             <div class="switchLang">
                <button class="flagButton" v-on:click="switchLang(); switchFlag()">
                    <img class="flag" v-if="picBool" src="@/assets/unionJack.jpg">
                    <img class="flag" v-else src="@/assets/sweFlag.png" > </button>
            </div>
        </div>

        <div v-show="showOverview === this.view" class="grid-containerOverview">

            <div class="overviewTop">
                <img class="logoOverview" src="@/assets/circle-cropped.png" >
                <div class="orderText">
                {{uiLabels.yourOrder}}
                </div>
                <div class="addBurger">
                <button class="burgerAdd" v-on:click="setView(showMenu); addBurger()">{{uiLabels.addBurger}}</button>
                </div>
            </div>

            <div class="burgerOverview">

                <div class="burgerScroll" v-for="burger in countAllBurgers"
                     :key="countAllBurgers.indexOf(burger)">
                    <h1 id="burgerNo"> Burger {{ burger.no + 1}} </h1>

                    <hr>

                    <div id=ingredientsInBurger v-for="countIng in burger.ingredientsShow" :key="burger.ingredientsShow.indexOf(countIng)">
                        {{countIng.count}}:  {{ countIng.name }}: {{countIng.ingPrice*countIng.count}} kr
                    </div>

                    <div id="burgerTotal">
                        <hr> <h4> {{uiLabels.price}} {{ burger.price }} :- </h4>
                    </div>


                    <!-- Mikaels kod för att visa en burgare + ingredienser, kan vara nice till kitchen vue? -->
<!--                    <div class="burgerScroll" v-for="(burger, key) in currentOrder.burgers" :key="key">-->
<!--                        {{key}}:-->
<!--                        <div v-for="(item, key2) in groupIngredients(burger.ingredients)" :key="key2">-->
<!--                             {{item.count}} x {{ item.ing['ingredient_' + lang] }}-->
<!--                          </div>-->
<!--                        {{burger.price}}-->
<!--                    </div>-->


                    <button id=editBurgerButton v-on:click="editBurger(burger, burger.no); checkBurger()"> {{uiLabels.editBurger}}</button>
                    <img id=deleteBurgerButton v-on:click="deleteBurger(burger.no, burger)" src="Delete-Button.png" width="35">
                    <button id=duplicateButton v-on:click="duplicateBurger(burger)"> <img src="Yum-Button.png" width="30"> {{uiLabels.dublicate}}</button>



                </div>

                <!--            <div class="burgerScroll" v-for="(burger, key) in currentOrder.burgers" :key="key">-->
                <!--                {{key}}:-->
                <!--                <span v-for="(item, key2) in burger.ingredients" :key="key2">-->
                <!--                    {{ item['ingredient_' + lang] }}-->
                <!--            </span>-->
                <!--                {{burger.price}}-->
                <!--            </div>-->


                <!-- Button that adds new burgers -->


              

            </div>

            <div class="overviewBottom">

               
                <div class="totalPrice" id="totalPrice">{{ uiLabels.total }}: {{totalPrice}} :-</div>

                <div class="placeOrder"><img id=PlaceOrderButton v-on:click="placeOrder()" src="PlaceOrder-Button.png" width="200" height="150"> </div>
            </div>
            
            <div class="overviewLang">
            
                 <div class="switchLang">
                    <button class="flagButton" v-on:click="switchLang(); switchFlag()">
                    <img class="flag" v-if="picBool" src="@/assets/unionJack.jpg">
                    <img class="flag" v-else src="@/assets/sweFlag.png" > </button>
                </div>
                
            </div>
        </div>

    </section>
</template>

<script>
//import the components that are used in the template, the name that you
// use for importing will be used in the template above and also below in
// components
import Ingredient from '@/components/Ingredient.vue'
import OrderItem from '@/components/OrderItem.vue'
//import methods and data that are shared between ordering and kitchen views
import sharedVueStuff from '@/components/sharedVueStuff.js'
import utilityMethods from '@/mixins/utilityMethods.js'
/* eslint-disable no-console */
/* instead of defining a Vue instance, export default allows the only
necessary Vue instance (found in main.js) to import your data and methods */
 export default {
     name: 'Ordering',
     components: {
         Ingredient,
         OrderItem,
     },
     mixins: [sharedVueStuff, utilityMethods], // include stuff that is used in both
     // the ordering system and the kitchen
     data: function () {
         return {
             chosenIngredients: [],
             price: 0,
             buttonClickable: false,
             orderNumber: "",
             name: "LoadingScreen",
             props: ["isLoading"],
             loading: false,
             count: 0,
             gluten: 0,
             milk: 0,
             vegan: 0,
             veganBool: true,
             glutenBool: true,
             milkBool: true,
             brodcategory: false,
             categoryNumber: 1,
             showFront: "showFront",
             showMenu: "showMenu",
             showOverview: "showOverview",
             view: "showFront",
             currentOrder: {
                 burgers: [],
                 editingBurger: false,
                 takeAway: false
             },
             picBool: false,
             totalPrice: 0,
         }
     },
     created: function () {
         this.$store.state.socket.on('orderNumber', function (data) {
             this.orderNumber = data;
         }.bind(this));
         this.$store.state.socket.on('')
     },
     computed: {
         countAllIngredients: function () {  //inkopierad från git, branch:severalburgers,kitchen
             // Räknar alla OLIKA ingredienser och hur många av dem
             let ingredientTuples = []
             for (let i = 0; i < this.chosenIngredients.length; i += 1) {
                 ingredientTuples[i] = {};
                 ingredientTuples[i].name = this.chosenIngredients[i]['ingredient_' + this.lang];
                 ingredientTuples[i].count = this.countNumberOfIngredients(this.chosenIngredients[i].ingredient_id);
                 ingredientTuples[i].ingPrice = this.chosenIngredients[i]['selling_price'];
             }
             var difIngredients = Array.from(new Set(ingredientTuples.map(o => o.name)))
                 .map(name => {
                     return {
                         name: name,
                         count: ingredientTuples.find(o => o.name === name).count,
                         ingPrice: ingredientTuples.find(o => o.name === name).ingPrice,
                     };
                 });
             console.log(difIngredients)
             return difIngredients;
         },
         countAllBurgers: function () { //liknande CountAllIngredients, fast för alla ing i en specifik burgare
             let severalBurgers = [];
             for (let j = 0; j < this.currentOrder.burgers.length; j += 1) {
                 let ingredientTuples = []
                 console.log(this.currentOrder.burgers[j].ingredients);
                 for (let i = 0; i < this.currentOrder.burgers[j].ingredients.length; i += 1) {
                     ingredientTuples[i] = {};
                     ingredientTuples[i].name = this.currentOrder.burgers[j].ingredients[i]['ingredient_' + this.lang];
                     ingredientTuples[i].count = this.countNumberOfBurgerIngredients(this.currentOrder.burgers[j].ingredients[i].ingredient_id, j);
                     ingredientTuples[i].ingPrice = this.currentOrder.burgers[j].ingredients[i]['selling_price'];
                     console.log(ingredientTuples[i].count)
                 }
                 var difIngredients = Array.from(new Set(ingredientTuples.map(o => o.name)))
                     .map(name => {
                         return {
                             name: name,
                             count: ingredientTuples.find(o => o.name === name).count,
                             ingPrice: ingredientTuples.find(o => o.name === name).ingPrice
                         };
                     });
                 severalBurgers[j] = {};
                 severalBurgers[j].no = j;
                 severalBurgers[j].ingredientsShow = difIngredients;
                 severalBurgers[j].ingredients = this.currentOrder.burgers[j].ingredients;
                 severalBurgers[j].price = this.currentOrder.burgers[j].price;
                 console.log(severalBurgers[j].ingredients)
             }
             console.log(severalBurgers)
             return severalBurgers
         }
     },
     methods: {
         takeAway: function() {
             this.currentOrder.takeAway = true;
         },
         addToBurger: function (item) {  //Lägger till ingrediens till burgare
             this.chosenIngredients.push(item);
             this.price += item.selling_price;
             this.totalPrice += item.selling_price;
         },
         delFromBurger: function (item) {  //Tar bort ingrediens
             this.chosenIngredients.splice(this.chosenIngredients.indexOf(item), 1);
             this.price -= item.selling_price;
             this.totalPrice -= item.selling_price;
         },
         ingredientCount: function (item) {  //Räknar ingredienserna.
             let counter = 0;
             for(let i = 0; i < this.chosenIngredients.length; i += 1) {
                 if (this.chosenIngredients[i] === item)
                     counter += 1;
             }
             return counter;
         },
         
         popUp: function() {
    //Show Loader
        this.loading = true;
        //Waste 5 seconds
        //Hide loader
      setTimeout(() => {
           this.loading = false;
        }, 3500)
    },
         
         checkBurger: function() {
             this.buttonClickable=false;
             for (let i = 0; i < this.chosenIngredients.length; i += 1) {
                if (this.chosenIngredients[i].category === 4) {
                    for (let i = 0; i < this.chosenIngredients.length; i += 1) {
                         if (this.chosenIngredients[i].category===1) {
                              this.buttonClickable=true;
                         }
                    }
                }
            }
        },
         addBurger: function() {   //Lägg till nny burgare
             this.buttonClickable=false;
             this.chosenIngredients = [];
         },
         
         
         addToOrder: function () {   //Lägg till burgaren till order!
             // Add the burger to an order array
             console.log(this.currentOrder)
             // kollar om currentOrder håller på att Edit en burgare, i så fall: uppdatera priset
             if (this.currentOrder.editingBurger) {
                 this.updatePrice()
             } else {                    // annars, alltså är det en ny burgare, lägger till burgare till ordern.
                 this.currentOrder.burgers.push({
                     ingredients: this.chosenIngredients.splice(0),
                     price: this.price,
                     editingThisBurger: false
                 });
                 this.currentOrder.editingBurger = false;
             }
             //set all counters to 0. Notice the use of $refs
             for (let i = 0; i < this.$refs.ingredient.length; i += 1) {
                 this.$refs.ingredient[i].resetCounter();
             }
             this.chosenIngredients = [];
             this.price = 0;
             this.currentOrder.editingBurger = false;
             this.view = "showOverview";
         },
         // countNumberOfIngredients: function (id) {   //OBS. Gammal ingredienscounter till sidovyn i ordering.
         //     let counter = 0;
         //     for (let order in this.chosenIngredients) {
         //         //console.log(order);
         //         // console.log(this.chosenIngredients[order])
         //         //let toppings = this.chosenIngredients[order];
         //         //console.log(toppings.length)
         //         //for (var i = 0; i < toppings.length; i += 1) ;
         //         //{
         //         if (this.chosenIngredients[order].ingredient_id === id) { //this.orders[order].status !== "done" &&
         //             counter += 1;
         //         }
         //         // }
         //     }
         //     return counter;
         // },
         countNumberOfBurgerIngredients: function (id, burgerNo) {  //Räknar ingredienserna per burgare till overview.
             let counter = 0;
             for (let item in this.currentOrder.burgers[burgerNo].ingredients) {
                 if (this.currentOrder.burgers[burgerNo].ingredients[item].ingredient_id === id) {
                     counter +=1;
                 }
             }
             return counter;
         },
         editBurger: function (burger, index) {   //Ändra din burgare genom denna. OBS! Om man ska ändra en duplicering
                                                  // så ändras alla burgare som är likadana. Fixa?
        
             this.currentOrder.burgers[index].editingThisBurger = true; //bestämmer att det är just denna burgaren i ordern som ändras
             this.currentOrder.editingBurger = true;  // Denna visar bara att användaren redigerar någon burgare
             this.chosenIngredients = burger.ingredients;
             this.price = burger.price;
             this.view = "showMenu"
         
         },
         deleteBurger: function (index, burger) {     //FUNKTION SOM TAR BORT BURGAREN.
              this.currentOrder.burgers.splice(index, 1);
              console.log(burger.price)
              console.log(this.currentOrder)
              this.totalPrice -= burger.price;
         },
         
           cancelOrder: function (index, burger) {     //FUNKTION SOM AVRBYTER ORDER
              this.currentOrder.burgers= [];
               this.currentOrder.editinBurger=false;
             this.chosenIngredients = [];
             this.buttonClickable= false;
             this.orderNumber= "";
             this.count= 0;
             this.gluten= 0;
             this.milk= 0;
             this.vegan= 0;
             this.veganBool= true;
             this.glutenBool= true;
             this.totalPrice = 0;
             this.currentOrder.burgers.price = 0;
             this.price = 0;
         },
         duplicateBurger: function (burger) {   // FUNKTION SOM FIXAR NY BURGARE EXAKT LIKADAN. Just nu problem med
                                                // att om man ska redigera en, redigeras ALLA duplicerade.
             var newBurger,
                 newBurger = burger;
             this.currentOrder.burgers.push(newBurger)
             this.totalPrice += burger.price;
         },
         updatePrice: function () {     //Här uppdateras priset
             for (let j = 0; j < this.currentOrder.burgers.length; j += 1) {
                 if (this.currentOrder.burgers[j].editingThisBurger) {
                     this.currentOrder.burgers[j].price = this.price;
                     this.currentOrder.burgers[j].editingThisBurger = false;
                 }
             }
         },
         setView: function (window) {       //Sätter vilken vy vi vill se.
             this.view = window;
         },
         setCategory: function (number) {
             this.categoryNumber = number;
         },
         showGlutenFree: function () {
             this.glutenBool = !this.glutenBool;
             if (!this.glutenBool) {
                 this.gluten = 1
             } else {
                 this.gluten = 0
             }
         },
         showMilkFree: function () {
             this.milkBool = !this.milkBool;
             if (!this.milkBool) {
                 this.milk = 1
             } else {
                 this.milk = 0
             }
         },
         showVeganFree: function () {
             this.veganBool = !this.veganBool;
             if (!this.veganBool) {
                 this.vegan = 1
             } else {
                 this.vegan = 0
             }
         },
         placeOrder: function () {
             // make use of socket.io's magic to send the stuff to the kitchen via the server (app.js)
             this.$store.state.socket.emit('order', this.currentOrder);
             console.log("PLACE ORDER")
             console.log(this.currentOrder)
         },
         switchFlag: function (){
                this.picBool = !this.picBool;
         },
     },
 }
</script>
<style scoped>
    /* scoped in the style tag means that these rules will only apply to elements, classes and ids in this template and no other templates. */
    
     @import url('https://fonts.googleapis.com/css?family=Dosis&display=swap');
    
    
    /* scoped in the style tag means that these rules will only apply to elements, classes and ids in this template and no other templates. */
    section {
          font-family: 'Dosis', sans-serif;
    }
    p {
font-family: 'Dosis', sans-serif;
}
/* Popup container */
.popup {
  position: absolute;
  z-index: 1;
  margin-left: 35%;
  margin-right: 35%;
  margin-top: 10%;
  display: inline-block;
  width: 20%;
  height: 20%;
  background-color: rgba(232, 232, 232, 0.8);
  border-radius: 2em;
  padding: 2em;
  border: 5px solid white;
}
  
    /* Popup text */
    .popUpText {
        font-size: 1.2vw;
    }
    
    .example-panel {
        left: 0;
        top: 0;
    }
    .ingredient {
        border: 1px solid grey;
        border-radius: 1.4em;
        text-align: center;
        align-items: center;
        padding: 0em 1em;
        height: 10vw;
        width: 11vw;
        font-size: 1vw;
        background-color: rgba(232, 232, 232, 0.92);
        color: dimgray;
    }

    .grid-container {
        display: grid;
        grid-template-columns: 74% 25%;
        grid-template-rows: 13% 55% 12% 15% 5%;
        grid-template-areas: "Top Burger" "OrderList Burger" "OrderList Total" "Done Done";
        background-image: url("wood.jpg");
        background-attachment: fixed;
        background-position: center;
        border-radius: 4em;
        border: 1px solid #FFF;
        width: 80%;
        height: 37em;
        padding: 3%;
        margin: auto;
    }
  .grid-containerFront {
        display: grid;
        grid-template-rows: 35% 60% 5%;
        grid-template-areas: "head" "mealLoc" "Lang";
        background-image: url("wood.jpg");
        background-attachment: fixed;
        background-position: center;
        border-radius: 4em;
        border: 1px solid #FFF;
        width: 80%;
        height: 37em;
        padding: 3%;
        margin: auto;
        color: white
    }
    .welcome {
        display:grid;
        grid-area: "head";
        grid-template-columns: 20% 80%;
        grid-template-rows: 100%;
        grid-template-areas: "logo" "headText";
        font-size: 6vw;
        overflow: inherit;
        text-align: center;
        font-weight: bold;
        
    }
    
    .logo {
        grid-area: "logo";
        width:100%;
        margin-top: -0.2em;
    }
    
    .headerText {
        grid-area: "headText";
        font-size: 6vw;
        overflow: inherit;
        text-align: center;
        font-weight: bold;
        
    }
    .logo {
    }
    .mealLocation {
        grid-area: "mealLoc";
        text-align: center;
        font-size: 3vw;
        background-color: rgba(232, 232, 232, 0.92);
        border-radius: 0.5em;
    }
    .switchLang {
        grid-area: "Lang";
        float: left;
        margin: 0.1em;
        margin-left: 1em;
        background-size: cover;

        
    }
    .flagButton {
        background-size: cover;
        background-color: rgba(1,1,1,0);
        border-color: rgba(1,1,1,0);

    }
    
    .flagButton:hover {
        cursor: pointer;
    }
    .flag {
        height: 4vw;
        width: 5.7vw;
        border-radius: 1em;
    }
    .mealButton {
        background-color: rgba(177, 160, 149, 0.65);
        font-family: 'Dosis', sans-serif;
        color: black;
        font-size: 4vw;
        border-radius: 0.25em;
        padding: 0.6em;
        margin-left: 1em;
        margin-right: 1em;
      
    }
    .mealButton:hover {
        background-color: black;
        color: white;
        cursor: pointer;
    }
    .Top {
        grid-area: Top;
        align-items: end;
    }
    .SwitchL {
        margin: -10%;
    }
    .OrderList {
        grid-area: OrderList;
        background-color: rgba(232, 232, 232, 0.92);
        padding: 4% 0% 0% 3%;
        margin-right: 5%;
        overflow-y: scroll;
        border-left: 3px solid #FFF;
        border-right: 3px solid #FFF;
        border-bottom: 3px solid #FFF;
        border-radius: 0em 0em 3em 3em;

    }
    .menuItems {
        display:grid;
        grid-gap: 2vw;
        grid-template-columns: repeat(auto-fill, 13vw);
        height: 10vw;

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

    }
    .Total h2 {
        margin: 0;
        padding: 0.5vw;
        padding-top: 0.5vw;
        line-height: 0.5em;
        display: inline-block;
        text-align: right;
        font-weight: bold;
        font-size: 3vw;
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
        font-size: 1.7vw;
    }
    /*
        border-bottom: 3px solid #FFF;
        border-right: 3px solid #FFF;
        border-left: 3px solid #FFF;
        border-top: 3px dotted #FFF;
        border-radius: 0em 0em 2em 2em;
        margin-left: -2em;
      }*/
    .Burger {
        padding: 1em;
        grid-area: Burger;
        border-top: 3px solid #FFFFFF;
        border-right: 3px solid #FFF;
        border-left: 3px solid #FFF;
        border-radius: 2em 2em 0 0;
        background-color: rgba(232, 232, 232, 0.92);
        font-size: 1.5vw;
        overflow-y: scroll;
    }

    .Burger button {
        font-size: 1vw;
    }


    /* Style the tab */
    .tab {
        grid-area: top;
        margin: 0% 5% 0% 0%;
        border-bottom: 3px solid #FFFFFF;
    }
    /* Style the buttons that are used to open the tab content */
    .tab button {
        background-color: rgba(232, 232, 232, 0.92);
        width: 16.66667%;
        height: 75px;
        font-size: 1.2vw;
        outline: none;
        cursor: pointer;
        padding: 2.3vw 0.9vw;
        text-align: center;
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
    .tab button:focus {
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
          font-family: 'Dosis', sans-serif;
        align-items: end;
    }
    /* För overview-sidan*/
    .grid-containerOverview {
        display: grid;
        grid-template-columns: 100%;
        grid-template-rows: 20% 55% 20% 5%;
        grid-template-areas: "Top" "Burgers" "Bottom" "Lang";
        margin-top: 3em;
        margin-left: 6em;
        background-image: url("wood.jpg");
        background-attachment: fixed;
        background-position: center;
        border-radius: 4em;
        border: 1px solid #FFF;
        width: 80%;
        height: 37em;
        padding: 3%;
        margin: auto;
        color: white;
    }
    .overviewTop {
        display:grid;
        grid-area: "head";
        grid-template-columns: 30% 40% 30%;
        grid-template-rows: 100%;
        grid-template-areas: "logoOverview" "orderText" "addBurger";
    }
    
    .orderText {
        font-size: 5.5vw;
        overflow: inherit;
        text-align: left;
        font-weight: bold;
        padding: 2%;
    }
    
    .logoOverview {
        grid-area: "logo";
        width:45%;
        margin-top: -0.8em;
        margin-left: 1em;
        overflow: inherit;

    }
    .burgerOverview {
        grid-area: Burgers;
        display: grid;
        grid-gap: 2vw;
        grid-auto-flow: column;
        overflow-x: scroll;
        text-align: center;
        margin: 1em;
    }
    .overviewBottom {
        grid-area: Bottom;
        display: grid;
        position: relative;
        align-items: center;
        grid-template-columns: 50% 50%;
        grid-template-rows: 100%;
        grid-template-areas: "totalPrice" "placeOrder";
    }
    
    .overviewLang{
        grid-area: "Lang";
    }
    
    #totalPrice {
/*
        text-align: center;
        font-size: 5vw;
        background-color: rgba(232, 232, 232, 0.92);
        border-radius: 0.5em;
        width: 60%;
        height: 70%;
        margin: 0.7em;
        margin-top: 0em;
        padding: 0px;
        float:left;
        color: black;
        
*/
        grid-area: "totalPrice";
        background-color: darkgray;
        padding: 1em;
        font-family: 'Dosis', sans-serif;
        color: black;
        font-size: 2.5vw;
        max-height: 56vw;
        height: auto;
        width: 22vw;
        margin-left: 1em;
        background-color: rgba(232, 232, 232, 0.92);
        font-family: 'Dosis', sans-serif;
        float: center;
        font-size: 2.5vw;
        border-radius: 0.2em 0.2em 0.2em 0.2em;
        border: 3px solid rgb(166, 166, 166);


    }
    #PlaceOrderButton {
        bottom: 2em;
        right: 5em;
        height: 10vw;
        width: 15vw;
        background-color:rgba(1,1,1,0);
        border-color: rgba(1,1,1,0);
        float: right;
        margin-left: 1em;
    }

    #PlaceOrderButton:hover {
        color: white;
        cursor: pointer;
}
    .burgerScroll {
        background-color: #1B686A;
        position: relative;
        text-align: left;
        width: 22vw;
        max-height: 320px;
        max-width: 200px;
        border-radius: 4vw;
        border: 5px solid #35A855;
        padding: 0 1em;
        font-size: calc(6e10px + 1vw - 6e10px);
    }
#burgerNo {
    text-align: center;
}
#ingredientsInBurger {
    overflow-x: scroll;
}
#burgerTotal {
    position: absolute;
    bottom: 4em;
}
    #deleteBurgerButton {
        position: absolute;
        top: -2px;
        right: -2px;
    }
    
     #cancelOrder {
        position: absolute;
        top: 1%;
        right: 6%;
        cursor: pointer;
         width: 5.5vw;
    }
#editBurgerButton {
    width: 100%;
    background-color: #82ceab;
    margin-left: -5%;
    position: absolute;
    bottom: 0px;
    height: 50px;
    border-top: 5px solid #35A855;
    border-color: #35A855;
    border-radius: 0em 0em 3.5vw 3.5vw;
       font-family: 'Dosis', sans-serif;
}
#duplicateButton {
    position: absolute;
    bottom: 6vw;
    right: 1vw;

    font-size: 1.1vw;
}

    #duplicateButton img{
        height: 1.5vw;
        width: 1.9vw;
    }
    .burgerAdd {
        /* grid-area: Done; */
        grid-area: "addBurger";
        background-color: darkgray;
        padding: 1em;
        font-family: 'Dosis', sans-serif;
        color: black;
        font-size: 2.5vw;
        max-height: 56vw;
        height: auto;
        width: 18vw;

        margin-top: -0.6em;
        background-color: rgb(255, 224, 102);
        font-family: 'Dosis', sans-serif;
        float: right;
        cursor: pointer;
        font-size: 2.5vw;
        border-radius: 0.2em 1em 0.2em 0.2em;
        border: 3px solid rgb(255, 179, 26);
    }
    
    .loader {
          background-color: #63ab97;
          bottom: 0;
          color: white;
          display: block;
          font-size: 32px;
          left: 0;
          overflow: hidden;
          padding-top: 10vh;
          position: fixed;
          right: 0;
          text-align: center;
          top: 0;
    }
        .burgerAdd:hover {
        background-color: rgb(255, 204, 0);
        color: white;
        cursor: pointer;
    }
.fadeout {
  animation: fadeout 2s forwards;
}
@keyframes fadeout {
  to {
    opacity: 0;
    visibility: hidden;
  }
}
    

    /*Designing of "Next"-button*/
    .nextPage {
        grid-area: Done;
        margin-left: 80%;
        background-color: rgba(135, 211, 124, 1);
        margin-top: 0.5em;
        font-family: 'Dosis', sans-serif;
        float: right;
        cursor: pointer;
        font-size: 2.5vw;
        width: 20%;
        height: 80%;
        border-radius: 0.2em 0.2em 1em 0.2em;
        border: 3px solid rgba(30, 130, 76, 1);
    }
    .nextPageNotClick {
        background-color: rgba(135, 211, 124, 0.9);
        margin-top: 0.5em;
        font-family: 'Dosis', sans-serif;
        float: right;
        font-size: 2em;
        width: 20%;
        height: 80%;
        border-radius: 0.2em 0.2em 1em 0.2em;
        border: 3px solid rgba(30, 130, 76, 1);
    }
    /* Designing of Foodfilter*/
    .label__checkbox {
      display: none;
}
    .positionVegan {
       margin-left: 30%;
    }
    .positionGluten {
       margin-left: 40%;
    }
     .positionMilk {
       margin-left: 50%;
    }
.label__check {
  display: block;
  position: absolute;
  border-radius: 50%;
  border: 5px solid rgba(0,0,0,0.1);
  background: rgba(255,255,255, 0.9);
  width: 5vw;
  height: 5vw;
  font-size: 1.4vw;
  cursor: pointer;
  transition: border .001s ease;
  text-align: center;
  margin-top: 2%;
    line-height: 1;
}
.label__checkbox:checked + .label__text .label__check {
  animation: check .2s cubic-bezier(0.895, 0.030, 0.685, 0.220) forwards;
}
.center {
  transform: translate(-50%,-50%);
}
@keyframes icon {
  from {
    opacity: 0;
    transform: scale(0.3);
  }
  to {
    opacity: 1;
    transform: scale(1)
  }
}
@keyframes check {
  0% {
      width: 2.5vw;
      height: 2.5vw;
    border-width: 5px;
      font-size: 0px;
  }
  10% {
      width: 2.5vw;
      height: 2.5vw;
    opacity: 0.1;
    background: rgba(0,0,0,0.2);
    border-width: 15px;
      font-size: 0.4vw;
  }
  12% {
      width: 2.5vw;
      height: 2.5vw;
    opacity: 0.4;
    background: rgba(0,0,0,0.1);
    border-width: 0;
      font-size: 0.7vw;
  }
  50% {
      width: 3.5vw;
      height: 3.5vw;
    background: rgba(144, 198, 149, 0.6);
    border: 1px solid rgba(38, 166, 91, 1);
      font-size: 1vw;
  }
  100% {
      width: 5vw;
      height: 5vw;
    background: rgba(144, 198, 149, 0.9);
    border: 4px solid rgba(38, 166, 91, 1);
    text-align: center;
  }
}
    
/* DESIGN FOR APP */    
@media only screen and (max-width: 500px)  {
     
 
    /*FÖRSTA SIDAN*/
      .welcome {
        font-size: 8vw;
        overflow: inherit;
        text-align: center;
        font-weight: bold;
        padding: 2%;
    }
    
    
        .mealLocation {
        text-align: center;
        font-size: 6vw;
        background-color: rgba(232, 232, 232, 0.92);
        border-radius: 1em;
        width: 90%;
        margin-left: auto;
        margin-right: auto;
        padding: 0px;
    }
    
        .mealButton {
        background-color: rgba(177, 160, 149, 0.65);
        font-family: 'Dosis', sans-serif;
        color: black;
        font-size: 4vw;
        border-radius: 0.25em;
        width: 50%;
        margin: 3%;
      
    }
    

    /*ORDERING VIEW*/
    
    /*ny grid-design*/
    .grid-container {
       display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: 0.4fr 1.7fr 0.4fr 1.5fr 0.5fr 0.5fr;
        grid-column-gap: 0px;
        grid-row-gap: 0px;
    } 

    
/*Top, Done, Burger, total och nextpage blir så höga som jag sätter dom på "grid-template-rows" men INTE orderlist :(*/
    .Top {
        grid-area: 1 / 1 / 2 / 2; 
    }
    
    
    .OrderList {
        height: 100%;
        grid-area: 2 / 1 / 3 / 2;
        padding: 4% 0% 0% 3%;
        overflow-y: scroll;


    }
    
    
      .Done {
        grid-area: 3 / 1 / 4 / 2;
      
    }
    
  
    
    
    .Burger {
        font-size: 3vw;
        grid-area: 4 / 1 / 5 / 2; 

        
    }
    
    .Total {
        font-size: 2vw;
        grid-area: 5 / 1 / 6 / 2;

    }
  
      .flag {
        height: 4vw;
        width: 5.8vw;
        border-radius: 0.3em 0.3em 0.3em 0.3em;
    }
    
    
    .nextPage {
        grid-area:  6 / 1 / 7 / 2;
        background-color: rgba(135, 211, 124, 1);
        margin-bottom: 0px;
        font-family: 'Dosis', sans-serif;
        cursor: pointer;
        margin-left: 0px;
        font-size: 5vw;
        width: 100%;
        height: 10vw;
        border-radius: 0.2em 0.2em 4em 4em;
        border: 3px solid rgba(30, 130, 76, 1);
    }
    
    
      .ingredient {
        height: 20vw;
        width: 22vw;
        font-size: 3vw;
      
    }
    
      .menuItems {
        grid-gap: 20vw;
        grid-template-columns: repeat(auto-fill, 13vw);
        height: 10vw;

    }
    
    
       .tab button {
        height: 10vw;
        font-size: 3.2vw;
   
    }
    
    .label_check {
        width: 8vw;
        height: 8vw;
    }
    
 
    }
        
        

    
    
    
</style>
<template>
    <div class="ingredient">

            <h3 class="head"> {{item["ingredient_"+ lang]}} </h3>

        <div class="buttons">

            <div v-if="item.category != 4">
                
                <button id="dec" v-on:click="decrementCounter($event)" :disabled="itemCount === 0">-</button>

               <button class="count"> {{  itemCount  }}</button>
                <button id="inc" v-on:click="incrementCounter($event)">+</button>
                <br>
            </div>


            <div class="radioButton" v-else-if="item.category == 4 && itemCount == 1">

                <input type="radio" name="radioButton" v-on:click="breadItem()" :disabled="itemCount >=1"
                       checked="checked">
            </div>

            <div class="radioButton" v-else>

                <input type="radio" name="radioButton" id="breadBox" v-on:click="breadItem()" :disabled="itemCount >=1">
            </div>



            </div>

     

        <h3 class="price">{{item.selling_price}}:- </h3>

        <img class="image" v-if="item.img != undefined" :src="require('../assets/' + item.img)" width="60" height="30"/>


        <div class="filter">
            <span v-if="item.gluten_free"> <img class="gluten" src="@/assets/gluten2.png" height="33"> </span>
            <span v-if="item.vegan"> <img class="vegan" src="@/assets/vegan2.png" height="30"> </span>
            <span v-if="item.milk_free"> <img class="milk" src="@/assets/milk2.png" height="30"> </span>
        </div>

        <!--          GAMMAL KOD           -->
        <!--    <label>-->
        <!--        -->

        <!--      {{item["ingredient_"+ lang]}}, {{item.selling_price}}:-, &lt;!&ndash;{{item.stock}} pcs &ndash;&gt;-->

        <!--    <br>-->
        <!--    <button v-on:click="incrementCounter" :disabled="item.stock === 0">+</button>-->
        <!--    {{ counter }}-->
        <!--    </label>-->

        <!--    <button v-on:click="decrementCounter"  :disabled="counter === 0">-</button>-->

    </div>
</template>
<script>
    /* eslint-disable no-console */
    export default {
        name: 'Ingredient',
        props: {
            item: Object,
            lang: String,
            itemCount: Number
        },
        data: function () {
            return {};
        },
        methods: {
            // incrementCounter: function () {          //////////////////////////// GAMLA KODER!
            //   this.counter += 1;
            //   // sending 'increment' message to parent component or view so that it
            //   // can catch it with v-on:increment in the component declaration
            //   console.log(this.item)
            //   this.$emit('increment');
            //   //this.$emit('counter', this.counter)
            //
            // },
            //
            // decrementCounter: function () {
            //   this.counter -= 1;
            //
            //   // sending 'increment' message to parent component or view so that it
            //   // can catch it with v-on:increment in the component declaration
            //   this.$emit('decrement');
            // },
            breadItem: function () {
                // sending 'pick' message to parent component or view so that it
                // can catch it with v-on:pick in the component declaration
                this.$emit('bread', this.item);
            },

            incrementCounter: function (ev) {
                if (ev.target.id === "inc") {
                    // sending 'increment' message to parent component or view so that it
                    // can catch it with v-on:increment in the component declaration
                    this.$emit('increment');
                }
            },
            decrementCounter: function (ev) {
                if (ev.target.id === "dec") {
                    // sending 'increment' message to parent component or view so that it
                    // can catch it with v-on:increment in the component declaration
                    this.$emit('decrement');
                }
            },
            resetCounter: function () {
                // this.counter = 0;
            },
            updateCounter: function () {
                console.log(this.counter)
                console.log(this.item)
            }
        }
    }
</script>
<style scoped>

    @import url('https://fonts.googleapis.com/css?family=Dosis&display=swap');

    p {
        font-family: 'Dosis', sans-serif;
    }

    .ingredient {
        display: grid;
        grid-template-columns: 40% 60%;
        grid-template-rows: 50% 30% 20%;
        grid-column-gap: 0px;
        grid-row-gap: 0px;
        
    }

   

    .buttons {
   grid-area: 3 / 1 / 4 / 3;

    }

    .head {
             grid-area: 1 / 2 / 2 / 3; 
            color: black;
            opacity: 0.7;

    }

 

    #inc {
        display: inline-block;
        cursor: pointer;
        background-color: rgba(135, 211, 124, 1);
        border-radius: 0em 1em 1em 0em;
        border-top: 0.11vw solid rgba(30, 130, 76, 1);
        border-right: 0.11vw solid rgba(30, 130, 76, 1);
        border-bottom: 0.11vw solid rgba(30, 130, 76, 1);
        border-left: 0.11vw solid rgba(135, 211, 124, 1);
        color: black;
        font-size: 1.5vw;
        width: 25%;
        height: 2vw;
        margin-bottom: 1vw;
        padding-bottom: 0.2vw;
        opacity: 0.7;
    }

    .count {
       display: inline-block;
        background-color: rgb(168, 223, 159);
      
        border: 0.11vw solid  rgba(30, 130, 76, 1);
        color: black;
        font-size: 1.5vw;
        width: 39%;
        height: 2vw;
        padding-bottom: 0.2vw;
        margin-bottom: 1vw;
        opacity: 0.7;
    }
    
    #dec {
        display: inline-block;
        cursor: pointer;
        background-color: rgba(135, 211, 124, 1);
        border-radius: 1em 0em 0em 1em;
        border-top: 0.11vw solid rgba(30, 130, 76, 1);
        border-left: 0.11vw solid rgba(30, 130, 76, 1);
        border-bottom: 0.11vw solid rgba(30, 130, 76, 1);
        border-right: 0.11vw solid rgba(135, 211, 124, 1);
        opacity: 0.7;
        color: black;
        font-size: 1.5vw;
        width: 25%;
        height: 2vw;
        margin-bottom: 1vw;
        padding-bottom: 0.2vw;
    }
    .radioButton input[type='radio'] {
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
        -webkit-appearance: none;
        -moz-appearance: none;
        appearance: none;
        background: white;
        width: 2.3vw;
        height: 1.8vw;
        margin-bottom: 8vw;
        margin-left: 0.3em;
        margin-top: 0.2em;
        transform: scale(5.8);
        border-radius: 0.2vw;
        -webkit-transition: all 0.2s;
        opacity: 0;
    }
    .radioButton input[type='radio']:checked {
        background: rgba(135, 211, 124, 1);
        border: 1px rgba(30, 130, 76, 1);
        border-style: double;
        opacity: 0.4;
    }
    .buttons input:hover {
        background: rgba(135, 211, 124, 1);
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
        transform: scale(6);
        border-radius: 0.2vw;
        -webkit-transition: all 0.2s;
        opacity: 0.2;

    }


    .price {
        grid-area: 2 / 1 / 3 / 2; 
        padding-bottom: 1vw;

    }

    .image {
        grid-area: 1 / 1 / 2 / 2;
        height: 2vw;
        width: 4vw;
    }

    .filter {
     grid-area: 2 / 2 / 3 / 3;
     padding-bottom: 1vw;
    }

    .filter img {
        height: 2vw;
        width: 2vw;
    }

    
    /* KOD FÖR APP*/
    @media only screen and (max-width: 500px) {


        .image {
            height: 11vw;
            width: 11vw;
        }
        
        .head {
            font-size: 3.4vw;
        }
        
      
        
    #inc {
        display: inline-block;
       cursor: pointer;
        font-size: 4vw;
        width: 25%;
        height: 4.5vw;
        margin-bottom: 1.5vw;
        padding-bottom: 0.2vw;
        text-align: center;
        border-top: 0.21vw solid rgba(30, 130, 76, 1);
        border-right: 0.21vw solid rgba(30, 130, 76, 1);
        border-bottom: 0.21vw solid rgba(30, 130, 76, 1);
        border-left: 0.21vw solid rgba(135, 211, 124, 1);
        
    }

    .count {
       display: inline-block;
        font-size: 4vw;
        width: 39%;
        height: 4.5vw;
        margin-bottom: 1.5vw;
        padding-bottom: 1vw;
        border: 0.21vw solid  rgba(30, 130, 76, 1);
    }
    
    #dec {
         display: inline-block;
        cursor: pointer;
        color: rgba(103, 87, 76, 1);
        font-size: 4vw;
        width: 25%;
        height: 4.5vw;
        background-color: rgba(135, 211, 124, 1);
        border-radius: 1em 0em 0em 1em;
        border-top: 0.21vw solid rgba(30, 130, 76, 1);
        border-left: 0.21vw solid rgba(30, 130, 76, 1);
        border-bottom: 0.21vw solid rgba(30, 130, 76, 1);
        border-right: 0.21vw solid rgba(135, 211, 124, 1);
        margin-bottom: 1.5vw;
        padding-bottom: 0.2vw;
        
    }
        .radioButton input[type='radio'] {
            -webkit-box-sizing: border-box;
            box-sizing: border-box;
            -webkit-appearance: none;
            -moz-appearance: none;
            appearance: none;
            background: white;
            width: 2.3vw;
            height: 1.73334vw;
            margin-bottom: 21vw;
            margin-left: auto;
            margin-top: auto;
            margin-right: 0px;
            transform: scale(15);
            border-radius: 0.2vw;
            -webkit-transition: all 0.2s;
            opacity: 0;
        }
        .radioButton input[type='radio']:checked {
            border-style: double;
            opacity: 0.2;

        }

        
        
        .price {
            font-size: 3.4vw;
        }
        
        .filter img {
            height: 5vw;
            width: 5vw;
            padding-bottom: 0.3vw;
    }
    }


</style>
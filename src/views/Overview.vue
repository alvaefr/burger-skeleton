<template>
    <section class="grid-container">
        <div class="Top">
            <h1> Din beställning </h1>
        </div>

        <div class="Burgers">

            <OrderItemToPrepare
                    class="OrderItemToPrepare"
                    v-for="order in orders"
                    :order="order"
                    :ui-labels="uiLabels"
                    :lang="lang">

                <!--v-on:done="changeOrder(key)"-->
                <!--:order-id="key" : denna säger vilken order det är
                :key="key"
                 v-if="order.status !== 'done'"-->
            </OrderItemToPrepare>

        </div>


            <div class="Bottom"> hej hej </div>
    </section>

</template>

<script>

    import Ingredient from '@/components/Ingredient.vue'
    import OrderItemToPrepare from '@/components/OrderItemToPrepare.vue'
    import OrderItem from '@/components/OrderItem.vue'
    import sharedVueStuff from '@/components/sharedVueStuff.js'
    import Ordering from './Ordering.vue'


    export default {
        name: 'Overview',
        components: {
            Ingredient,
            OrderItem,
            Ordering,
            OrderItemToPrepare
        },
        mixins: [sharedVueStuff],

        data: function(){
            return {
                chosenIngredients: [],
                price: 0
            }
        }
    }

</script>

<style scoped>

    .grid-container {
        display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: 0.5fr 1.7fr 0.8fr;
        grid-template-areas: "Top" "Burgers" "Bottom";
        margin-top: 3em;
        margin-left: 6em;
    }

    .Burgers {
        grid-area: Burgers;
        display: grid;
        grid-gap: 25px;
        width: 65em;
        grid-auto-flow: column;
        overflow-x: scroll;
        text-align: center;
        margin-top: 1em;
    }

    .OrderItemToPrepare {
        background-color: aquamarine;
        width: 15em;
        height: 15em;
        border-radius: 2em;
        padding: 2em;
    }

    .Top { grid-area: Top; }

    .Bottom { grid-area: Bottom; }



</style>

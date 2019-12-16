<template>
    <section class="grid-container">
        <div class="Top">
            <h1> Din beställning </h1>
        </div>

        <div class="Burgers">

        {{burgers}

            <div v-for="(burger, key) in currentOrder.burgers" :key="key">
                {{key}}:
                <span v-for="(item, key2) in burger.ingredients" :key="key2">
          {{ item['ingredient_' + lang] }}
        </span>
                {{burger.price}}
            </div>

<!--            <OrderItemToPrepare-->
<!--                    class="OrderItemToPrepare"-->
<!--                    v-for="order in orders"-->
<!--                    :order="order"-->
<!--                    :ui-labels="uiLabels"-->
<!--                    :lang="lang">-->

<!--                &lt;!&ndash;v-on:done="changeOrder(key)"&ndash;&gt;-->
<!--                &lt;!&ndash;:order-id="key" : denna säger vilken order det är-->
<!--                :key="key"-->
<!--                 v-if="order.status !== 'done'"&ndash;&gt;-->
<!--            </OrderItemToPrepare>-->
            <OrderItemToPrepare
                    v-for="(order, key) in orders"
                    :order-id="key"
                    :order="order"
                    :ui-labels="uiLabels"
                    :lang="lang"
                    :key="key">
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
//        props: {
//            order: Object
//        },
        components: {
            Ingredient,
            OrderItem,
            Ordering,
            OrderItemToPrepare,
        },
        mixins: [sharedVueStuff],

        data: function(){
            return {
                chosenIngredients: [],
                price: 0,
                burgers: []
            }
        },

    }

</script>

<style scoped>

    .grid-containerOverview {
        display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: 0.5fr 1.7fr 0.8fr;
        grid-template-areas: "Top" "Burgers" "Bottom";
        margin-top: 3em;
        margin-left: 6em;
    }

    .burgerOverview {
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

    .overviewTop { grid-area: Top; }

    .overviewBottom { grid-area: Bottom; }



</style>

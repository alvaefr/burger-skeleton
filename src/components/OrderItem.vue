<template>
<!--	<div>      								GAMLA KODEN										-->
<!--		{{order.orderId}}-->
<!--		<div v-for="(burger, key) in order.burgers" :key="key">-->
<!--		&lt;!&ndash; //{{uiLabels.ingredients}}: {{ burger.ingredients.map(item=>item["ingredient_"+ lang]).join(", ") }} &ndash;&gt;-->
<!--			<div v-for="(ingredient) in burger.ingredients">-->
<!--				<div v-if="(categoryNum.includes(ingredient.category))">-->
<!--							name: {{ingredient.ingredient_en}}-->
<!--							category: {{ingredient.category}}-->
<!--							ingredient_id: {{ingredient.ingredient_id}}-->
<!--							number: {{countNumberOfIngredients(ingredient.ingredient_id)}}-->
<!--			</div>-->
<!--		</div>-->
<!--	</div>-->
<!--	&lt;!&ndash; <div v-for="countIng in countAllIngredients"-->
<!--			v-if="countIng.count>0"-->
<!--			:key="countAllIngredients.indexOf(scountIng)">-->
<!--		{{countIng.name}}: {{countIng.count}}-->
<!--	</div> &ndash;&gt;-->
<!--	<p id="demo"></p>-->
<!--	</div>-->

		<div class = 'grid-item'>
		<div id = 'orderID'> #{{order.orderId}} </div>
		<!-- 						De två raderna under här visar om det är takeAway eller äta på restaurangen 			-->
		<div id="takeAway" v-if="order.takeAway === true"> Take Away </div>
		<div id="takeAway" v-if="order.takeAway === false"> Eat Here </div>

		<div v-for="(burger, key) in order.burgers" :key="key">

			<div id = 'burgerID'> Burger {{key + 1}}: </div>
			<div v-for="(ing, key2) in groupIngredients(burger.ingredients)" :key="key2">
					<div v-if="(categoryNum.includes(ing.category_num))">
				{{ ing.count + " x " + ing.ing["ingredient_" + lang]}}
			</div>
			</div>
		</div>
				</div>
</template>
<script>

	import utilityMethods from '@/mixins/utilityMethods.js'
export default {
	name: 'OrderItem',
	props: {
		uiLabels: Object,
		order: Object,
		lang: String,
		categoryNum: Array
	},
	mixins: [utilityMethods],
	methods: {
		countNumberOfIngredients: function (id) {
	// 	let counter = 0;
	// 	for (let burger in this.order.burgers) {
	// 		for (let ingredient in burger.ingredients) {
	// 							if (ingredient.ingredient_id === id){
	// 				counter +=1;
	// 			}
	// 				}
	// 			}
	// 	return counter;
	// }
	let counter = 0;
	//Now we have an array of burgers in an order so we need to add a loop
	let burgers = this.order.burgers;
	for (let j = 0; j < burgers.length; j += 1) {
		for (let i = 0; i < burgers[j].ingredients.length; i += 1) {
			if (burgers[j].ingredients[i].ingredient_id === id) {
				counter +=1;
			}
		}
	}
	console.log(order)
	return counter;
	}
}
	}
	</script>
<style scoped>

	.grid-item {
	  background-color: darkgrey;
	  border: 1px solid rgba(0, 0, 0, 0.8);
	  padding: 10px;
	  font-size: 30px;
	  text-align: center;
		font-size: 12pt;
	}
#orderID {
	text-align: right;
	font-size: 15pt;
	      font-weight: bold;
}
#burgerID {
	      font-weight: bold;
					text-align: left;
}
</style>

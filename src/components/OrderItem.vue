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
	<div>
		{{order.orderId}}
		<div v-for="(burger, key) in order.burgers" :key="key">
			{{uiLabels.ingredients}}:
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
	return counter;
	}
}
	}
	</script>
<style scoped>

</style>

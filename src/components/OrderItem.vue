<template>
	<div>
		{{order.orderId}}
		<div v-for="(burger, key) in order.burgers" :key="key">
		<!-- //{{uiLabels.ingredients}}: {{ burger.ingredients.map(item=>item["ingredient_"+ lang]).join(", ") }} -->
			<div v-for="(ingredient) in burger.ingredients">
				<div v-if="(categoryNum.includes(ingredient.category))">
							name: {{ingredient.ingredient_en}}
							category: {{ingredient.category}}
							ingredient_id: {{ingredient.ingredient_id}}
							number: {{countNumberOfIngredients(ingredient.ingredient_id)}}
			</div>
		</div>
	</div>
	<!-- <div v-for="countIng in countAllIngredients"
			v-if="countIng.count>0"
			:key="countAllIngredients.indexOf(scountIng)">
		{{countIng.name}}: {{countIng.count}}
	</div> -->
	<p id="demo"></p>
	</div>
</template>
<script>
export default {
  name: 'OrderItem',
  props: {
    uiLabels: Object,
    order: Object,
    orderId: String,
    lang: String,
		categoryNum: Array
  },
	data: function () {
	return {
	};
},
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

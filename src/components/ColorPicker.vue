<template>
	<div>
		<div class="cards">
			<color-card
				v-for="card in 5"
				:key="card.id"
				:startGenerateCards="startGenerateCards"
				@getGeneratedCard="getGeneratedCard"
			/>
		</div>
	</div>
</template>

<script>
import ColorCard from "@/components/UI/ColorCard.vue";

export default {
	components: {
		ColorCard,
	},
	data() {
		return {
			startGenerateCards: false,
			cards: [],
		};
	},
	methods: {
		getGeneratedCard(card) {
			this.cards.push(card);
		},
	},
	mounted() {
		this.startGenerateCards = true;
	},
};
</script>

<style lang="scss">
</style>
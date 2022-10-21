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
		setRandomHEXColors() {
			this.cards.forEach((card) => {
				if (!card.pinned) {
					card.colorHEX = (
						"00000" + ((Math.random() * (1 << 24)) | 0).toString(16)
					).slice(-6);
				}
			});
		},
	},
	mounted() {
		this.startGenerateCards = true;

		document.addEventListener("keydown", (event) => {
			if (event.code.toLowerCase() === "space") {
				event.preventDefault();
				this.setRandomHEXColors();
			}
		});
	},
};
</script>

<style lang="scss">
.cards {
	width: 100vw;
	height: 100vh;
	display: flex;

	.color-card {
		flex-grow: 1;
	}
}
</style>
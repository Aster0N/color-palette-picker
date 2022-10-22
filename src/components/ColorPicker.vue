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
			isInitialLoad: true,
			cards: [],
			colors: [],
		};
	},
	methods: {
		getGeneratedCard(card) {
			this.cards.push(card);
			if (!this.isInitialLoad) {
				this.colors.push(card.colorHEX);
			} else {
				let colorEl = this.getColorsFromHash()[this.cards.length - 1];
				card.colorHEX = colorEl;
				this.colors.push(colorEl);
			}
		},
		setRandomHEXColors() {
			this.colors = this.isInitialLoad ? this.getColorsFromHash() : [];

			this.cards.forEach((card, index) => {
				if (!card.pinned) {
					let randomHEX = (
						"00000" + ((Math.random() * (1 << 24)) | 0).toString(16)
					).slice(-6);
					card.colorHEX = this.isInitialLoad
						? this.colors[index]
							? this.colors[index]
							: randomHEX
						: randomHEX;

					if (!this.isInitialLoad) {
						this.colors[index] = card.colorHEX;
					}
				}
			});
			this.updateColorsHash();
			this.isInitialLoad = false;
		},
		updateColorsHash() {
			document.location.hash = this.colors.join("-");
		},
		getColorsFromHash() {
			if (document.location.hash.length > 1) {
				return document.location.hash.substring(1).split("-");
			}
			return [];
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
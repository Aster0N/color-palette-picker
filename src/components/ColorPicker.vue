<template>
	<div>
		<div class="copied-message" v-show="showCopiedMessage">copied</div>
		<div class="cards">
			<color-card
				v-for="card in cardsNumber"
				:key="card.id"
				:startGenerateCards="startGenerateCards"
				@getGeneratedCard="getGeneratedCard"
				@setCopiedMessage="setCopiedMessage"
			/>
			<div class="backing" v-show="showCopiedMessage"></div>
			<div class="guid-info" v-show="openGuidInfo">
				<p class="guid-text">Hit the 'space' to change colors</p>
				<button class="guid-button" @click="openGuidInfo = false">
					ok
				</button>
			</div>
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
			cardsNumber: 5,
			startGenerateCards: false,
			isInitialLoad: true,
			cards: [],
			colors: [],
			showCopiedMessage: false,
			openGuidInfo: false,
		};
	},
	methods: {
		setCopiedMessage() {
			this.showCopiedMessage = true;
			setTimeout(() => {
				this.showCopiedMessage = false;
			}, 300);
		},
		getGeneratedCard(card) {
			if (!this.isInitialLoad) {
				this.colors.push(card.colorHEX);
			} else {
				let colorsFromHash = this.getColorsFromHash(card.colorHEX);
				if (colorsFromHash.length) {
					let colorEl = colorsFromHash[this.cards.length];
					card.colorHEX = colorEl;
					this.colors.push(colorEl);
				} else {
					this.colors.push(card.colorHEX);
				}
			}
			this.cards.push(card);
			if (this.cards.length == this.cardsNumber) {
				this.updateColorsHash();
			}
		},
		setRandomHEXColors() {
			this.colors = this.getColorsFromHash();

			this.cards.forEach((card, index) => {
				if (!card.pinned) {
					let randomHEX = (
						"00000" + ((Math.random() * (1 << 24)) | 0).toString(16)
					).slice(-6);

					card.colorHEX = randomHEX;
				}
				this.colors[index] = card.colorHEX;
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
		this.openGuidInfo = true;

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
.copied-message {
	position: absolute;
	top: 50px;
	left: 50%;
	width: 200px;
	text-align: center;
	transform: translateX(-50%);
	font-size: 1.3em;
	border-radius: 5px;
	padding: 20px;
	text-transform: uppercase;
	pointer-events: none;
	background-color: rgb(80, 226, 163);
	z-index: 1;
}

.cards {
	width: 100vw;
	height: 100vh;
	display: flex;

	.color-card {
		flex-grow: 1;
	}

	.backing {
		position: absolute;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		background-color: rgba(0, 0, 0, 0.8);
	}

	.guid-info {
		padding: 20px;
		font-size: 1.3em;
		position: absolute;
		top: 30px;
		left: 50%;
		transform: translateX(-50%);
		border-radius: 5px;
		background-color: rgb(245, 245, 245);

		.guid-button {
			border: none;
			outline: none;
			padding: 5px 10px;
			margin: 0;
			cursor: pointer;
			text-transform: uppercase;
			background-color: rgb(28, 28, 28);
			color: rgb(245, 245, 245);
		}
	}
}
</style>
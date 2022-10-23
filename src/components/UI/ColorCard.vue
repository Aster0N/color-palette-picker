<template>
	<div
		class="color-card"
		:style="{ 'background-color': `#${this.card.colorHEX}` }"
	>
		<span class="color-text" @click="copyColorToClipboard"
			>#{{ card.colorHEX }}</span
		>
		<button class="pin-card-btn" @click="card.pinned = !card.pinned">
			<img :src="getThumbtackImage()" alt="thumbtack" />
		</button>
	</div>
</template>

<script>
import { v4 as uuidv4 } from "uuid";

export default {
	props: {
		startGenerateCards: {
			type: Boolean,
			required: true,
		},
	},
	data() {
		return {
			card: {},
		};
	},
	methods: {
		generateCard() {
			this.card = {
				id: uuidv4(),
				colorHEX: (
					"00000" + ((Math.random() * (1 << 24)) | 0).toString(16)
				).slice(-6),
				pinned: false,
			};
			this.$emit("getGeneratedCard", this.card);
		},
		getThumbtackImage() {
			var images = require.context("@/assets/images/", false, /\.svg$/);
			return this.card.pinned
				? images("./" + "thumbtack-shaded" + ".svg")
				: images("./" + "thumbtack" + ".svg");
		},
		copyColorToClipboard() {
			this.$emit("setCopiedMessage");
			return navigator.clipboard.writeText(`#${this.card.colorHEX}`);
		},
	},
	watch: {
		startGenerateCards(value) {
			if (value) {
				this.generateCard();
			}
		},
	},
};
</script>

<style lang="scss">
.color-card {
	cursor: pointer;
	flex: 1;
	border-bottom: 1px solid #000;
	display: flex;
	justify-content: space-evenly;
	align-items: center;
	flex-direction: column;

	.color-text,
	.pin-card-btn {
		background-color: rgba(255, 255, 255, 0.3);
		transition: all 0.3s ease-in-out;
		padding: 10px;

		&:hover {
			background-color: rgba(255, 255, 255, 0.6);
		}
	}

	.color-text {
		font-size: 1.3em;
		border-radius: 5px;
	}

	.pin-card-btn {
		cursor: pointer;
		border: none;
		border-radius: 5px;
		outline: none;
	}
}
</style>
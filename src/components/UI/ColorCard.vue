<template>
	<div
		class="color-card"
		:style="{ 'background-color': `#${this.card.colorHEX}` }"
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
	border-bottom: 1px solid #000;
	display: flex;
	justify-content: center;
	align-items: center;

	.pin-card-btn {
		cursor: pointer;
		border: none;
		border-radius: 5px;
		outline: none;
		padding: 5px;
		transition: all 0.3s ease-in-out;
		background: transparent;

		&:hover {
			background-color: rgba(255, 255, 255, 0.612);
		}
	}
}
</style>
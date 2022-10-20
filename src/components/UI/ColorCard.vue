<template>
	<div class="color-card">card</div>
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
	methods: {
		generateCard() {
			const card = {
				id: uuidv4(),
				color: Math.floor(Math.random() * 0xffffff).toString(16),
				pinned: false,
			};
			this.$emit("getGeneratedCard", card);
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
	padding: 50px;
	border: 1px solid #000;
}
</style>
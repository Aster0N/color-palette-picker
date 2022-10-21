<template>
	<div
		class="color-card"
		:style="{ 'background-color': `#${this.card.colorHEX}` }"
	></div>
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
}
</style>
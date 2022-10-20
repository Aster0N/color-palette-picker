<template>
	<div>
		<div class="cards">
			<color-card />
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
		};
	},
	mounted() {
		this.generateCards = true;
	},
};
</script>

<style lang="scss">
</style>
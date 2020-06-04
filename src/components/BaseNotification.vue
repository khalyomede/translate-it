<template lang="pug">
	div(:class="classes")
		slot
</template>

<script lang="ts">
	import Vue from "vue";
	export default Vue.extend({
		props: {
			opened: {
				type: Boolean,
				default: false,
			},
		},
		computed: {
			classes() {
				return {
					"notification": true,
					"rounded-sm": true,
					"bg-gray-800": true,
					"text-white": true,
					"p-2": true,
					"mb-8": this.opened,
					"fixed": true,
					"bottom-0": true,
					"transform": true,
					"translate-y-full": !this.opened,
					"transition-transform": true,
					"duration-150": true,
					"ease-out": true,
				};
			},
		},
	});
</script>

<style lang="sass" scoped>
	@import "tailwindcss/base"
	@import "tailwindcss/components"
	@import "tailwindcss/utilities"
</style>

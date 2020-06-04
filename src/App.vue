<template lang="pug">
	div#app.h-screen.w-screen.overflow-x-hidden
		.table-cell.w-screen.h-screen.align-middle(class="xl:p-64 lg:p-32 md:p-16 p-8")
			router-view
</template>

<style lang="sass">
@import "tailwindcss/base"
@import "tailwindcss/components"
@import "tailwindcss/utilities"
</style>

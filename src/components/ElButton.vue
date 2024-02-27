<template v-if="title">

<!-- @click="$modal.open(`modal-feedback`, { form: form })" -->

	<button
		v-if="submit"
		class="el-button"
		:class="{
			[themeClass]: `${themeClass}`
		}"
	>{{ title }}</button>

	<div
		v-else
		class="el-button"
		:class="{
			[themeClass]: `${themeClass}`
		}"
	>{{ title }}</div>

</template>

<script setup>

	defineProps(['title','submit','themeClass'])

</script>

<style lang="scss">

	.el-button{
		@apply py-[1.125rem] px-10 text-lg min-w-[15.75rem] inline-block align-top text-center font-semibold select-none
			rounded-2xl transition cursor-pointer
		;
		&.red{
			@apply bg-red-500 text-white hover:bg-black;
		}
		&.white{
			@apply bg-white text-black hover:bg-black hover:text-white;
		}
		&.blue{
			@apply bg-blue-500 text-white hover:bg-black;
		}
	}

</style>

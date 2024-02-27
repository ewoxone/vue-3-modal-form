<template>
	<div class="flex items-start select-none">

		<label class="flex items-center h-5">
			<input
				name="checkbox-policy"
				type="checkbox"
				class="form-checkbox border-[#727272] text-red-500 hover:text-black
					focus:ring-transparent rounded-sm cursor-pointer transition-colors"
				:class="{
					'!border-red-500': state === false
				}"
				aria-describedby="checkbox-policy-description"
				checked
				v-model="modelValue"
			>
		</label>

		<div
			class="ml-3 text-lg leading-[1.3] font-medium"
			:class="{
				'!text-red-500': state === false
			}"
		>
			Я согласен(а) на
			<a
				href="#"
				class="underline hover:no-underline"
				target="_blank"
			>обработку персональных данных</a>
		</div>

	</div>
</template>

<script setup>

	defineProps(['state']);

	const modelValue = defineModel();

</script>

<style lang="scss">

</style>
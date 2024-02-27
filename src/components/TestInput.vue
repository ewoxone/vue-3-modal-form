<script setup>

import ElValidFailedMsg from './ElValidFailedMsg.vue';

import { vMaska } from 'maska';

defineProps(['placeholder','inputPhone','state','validFailedMsg','inputClass']);

const modelValue = defineModel();

</script>

<template>
	<div>
		<input
			v-if="inputPhone"
			type="text"
			class="c-input form-input"
			:class="{
				'!border-red-500': state === false,
				[inputClass]: inputClass
			}"
			:placeholder="placeholder"
			v-maska
			data-maska="+7 (###) ### - ## - ##"
			v-model="modelValue"
		>
		<input
			v-else
			type="text"
			class="c-input form-input"
			:class="{
				'!border-red-500': state === false,
				[inputClass]: inputClass
			}"
			:placeholder="placeholder"
			v-model="modelValue"
		>

		<El-valid-failed-msg
			:msg="validFailedMsg"
		/>

	</div>
</template>

<style lang="scss">

	.c-input.form-textarea,
	.c-input.form-input{
		@apply
			block w-full text-lg text-black border-[#C4C4C4] rounded
			focus:border-black focus:ring-transparent placeholder:text-[#BFBBBC]
			px-5 py-2.5
		;
	}

</style>
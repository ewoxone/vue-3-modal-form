<script setup>
import ElValidFailedMsg from './ElValidFailedMsg.vue';
import { reactive } from 'vue';

defineProps(['isError','validFailedMsg','inputClass']);

const modelValue = defineModel();

const state = reactive({
	errMsg: ''
});

const onFileChange = async (e) => {

	const input = e.target;
	const file = input.files?.[0];

	state.errMsg = '';

	if (!file) {
		clearFile('Вы не выбрали файл, файл не загружен!');
		return;
	}

	function clearFile(msg) {
		state.errMsg = msg;
		input.value = '';
		modelValue.value = '';
	}

	if (file.size > 2 * 1024 * 1024) {
		clearFile('Файл должен быть не более 2MB!');
		return;
    }

	if (file.type.startsWith('image/') || file.type.endsWith('/pdf')) {
		// const formData = new FormData();
		// formData.append('file', file);
		// modelValue.value = formData;

		const toBase64 = file => new Promise((resolve, reject) => {
			const reader = new FileReader();
			reader.readAsDataURL(file);
			reader.onload = () => resolve(reader.result);
			reader.onerror = error => reject(error);
		});

		modelValue.value = {
			name: file.name,
			data: await toBase64(file)
		}

    } else {
		clearFile('Необходимо загрузить документ формата PDF или изображение!');
		return;
	}

}

</script>

<template>
	<div>
		<label class="block">
			<span class="sr-only">Choose file</span>
			<input
				type="file"
				ref="inputFile"
				class="
					block w-full text-sm text-slate-500
					file:mr-4 file:py-2 file:px-4
					file:rounded-full file:border-0
					file:text-sm file:font-semibold
					file:bg-red-500 file:text-white
					hover:file:bg-black file:transition-colors
					focus:outline-none file:cursor-pointer
				"
				:class="{
					'!text-red-500': isError === false,
					[inputClass]: inputClass
				}"
				@change="onFileChange($event)"
			/>
		</label>

		<El-valid-failed-msg
			:msg="state.errMsg || validFailedMsg"
		/>

	</div>
</template>

<style lang="scss">

</style>
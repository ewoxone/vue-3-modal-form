<script setup>
import TestInput from './TestInput.vue';
import TestInputFile from './TestInputFile.vue';
import ElCheckboxPolicy from './ElCheckboxPolicy.vue';
import ElButton from './ElButton.vue';
import OverlayLoader from './OverlayLoader.vue';
import FormResponse from './FormResponse.vue';
import TestError from './TestError.vue';

import { reactive, ref } from 'vue';
import { useVuelidate } from '@vuelidate/core';
import { required, helpers, email, sameAs } from '@vuelidate/validators';

const state = reactive({
	name: '',
	phone: '',
	email: '',
	file: {
		name: '',
		data: ''
	},
	policy: true,
})

const rules = {
	name: {
		required: helpers.withMessage('Введите ваше имя', required)
	},
	phone: {
		required: helpers.withMessage('Введите ваш телефон', required),
		regex: helpers.withMessage('Ваш телефон должен быть формата +7 (***) *** - ** - **', helpers.regex(/^\+7 \(\d\d\d\) \d\d\d - \d\d - \d\d$/))
	},
	email: {
		required: helpers.withMessage('Введите ваш Email', required),
		email: helpers.withMessage('Email должен быть вида например *****@mail.ru', email)
	},
	file: {
		required: helpers.withMessage('Необходимо прикрепить файл', required)
	},
	policy: {
		required,
		sameAsTrue: sameAs(true)
	}
}

const pending = ref(false);
const response = ref(null);
const error = ref(null);

const v$ = rules && state ? useVuelidate(rules, state) : null;

const onSubmit = async () => {

	// берем все поля, кроме телефона
	let body = {
		name: state.name,
		email: state.email,
		file: state.file,
		policy: state.policy,
	};

	// оставляем в телефоне только разрешенные символы
	body.phone = state.phone.replace(/[^+\d]/g, '');

	const data = fetch('https://jsonplaceholder.typicode.com/posts/', {
		method: "POST",
		headers: {
			'Content-type': 'application/json'
		},
		body: JSON.stringify(body)
	});

	const res = await data;

	if (!res?.ok) throw res;

	return res

}

const submitHandler = async () => {

	try {

		if (pending.value) return;

		error.value = null;

		if (v$) {
			const validateStatus = await v$.value.$validate();
			if (!validateStatus) return;
		}

		pending.value = true;

		response.value = await onSubmit();

	} catch (e) {
		error.value = e;
	} finally {
		pending.value = false;
	}

}

</script>

<template>

<form ref="form" @submit.prevent="submitHandler">

	<OverlayLoader v-if="pending" />

	<Transition name="fade-100" appear>
		<div v-if="!response">

			<TestInput
				placeholder="Имя"
				class="mb-4"
				v-model="state.name"
				:state="v$.name.$error ? false : null"
				:validFailedMsg="v$.name.$errors[0]?.$message"
			/>

			<TestInput
				placeholder="Телефон"
				class="mb-4"
				v-model="state.phone"
				:state="v$.phone.$error ? false : null"
				:validFailedMsg="v$.phone.$errors[0]?.$message"
				:inputPhone="true"
			/>

			<TestInput
				placeholder="Email"
				class="mb-4"
				v-model="state.email"
				:state="v$.email.$error ? false : null"
				:validFailedMsg="v$.email.$errors[0]?.$message"
			/>

			<TestInputFile
				class="mb-8"
				v-model="state.file"
				:isError="v$.file.$error ? false : null"
				:validFailedMsg="v$.file.$errors[0]?.$message"
			/>

			<TestError v-if="error" :error="error" />

			<ElButton
				title="Отправить"
				themeClass="red !max-w-none w-full"
				:submit="true"
				:disabled="pending"
			/>

			<ElCheckboxPolicy
				class="mt-8"
				v-model="state.policy"
				:state="v$.policy.$error ? false : null"
			/>

		</div>

		<FormResponse
			v-else
			:response="response"
		/>
	</Transition>

</form>

</template>
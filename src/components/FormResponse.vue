<template>
	<div
		v-if="response"
		class="text-center text-green-600"
	>

		<!-- Ответ от сервера может быть разный, так что оставляю для примера response, но пока для красоты сделаю статичное сообщение -->
		<!-- {{ response }} -->

		Форма отправлена, с вами свяжутся в ближайщее время!

	</div>
</template>

<script setup>

	defineProps(['response']);

</script>
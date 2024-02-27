<template>
	<div
		v-if="error"
		class="my-5 bg-gray-50 border border-gray-200 text-sm text-gray-600 rounded-md p-4"
	>
		{{ error.status }}
	</div>
</template>

<script setup>

defineProps(['error'])

</script>
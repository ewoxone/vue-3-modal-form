<script setup>
import IconDanger from './icons/IconDanger.vue';

defineProps(['msg']);
</script>

<template>
	<div
		v-if="msg"
		class="flex items-center text-red-500 text-sm mt-1"
	>
		<IconDanger class="icon mr-2 shrink-0"/>
		{{ msg }}
	</div>
</template>

<style lang="scss">

</style>
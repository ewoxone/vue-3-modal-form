<script setup>

import { ref } from 'vue';
import IconCross from './icons/IconCross.vue';
import ElButton from './ElButton.vue';

const visible = ref(false);

</script>

<template>

	<div class="min-h-screen flex items-center justify-center">
		<El-Button
			title="Обратный звонок"
			themeClass="blue"
			@click="visible = true"
		/>
	</div>

	<Teleport to="body">
		<Transition name="fade-100" appear>
			<div
				v-if="visible"
				class="w-full h-full fixed top-0 left-0 flex z-[110] overflow-x-hidden overflow-y-auto bg-[#262626] bg-opacity-40"
				@click.self="visible = false"
			>
				<div class="max-w-xs sm:max-w-[28.25rem] sm:w-full m-auto">
					<div class="relative p-10 bg-white">
						<slot />
						<div
							class="absolute right-0 top-0 text-red-500 p-2.5 leading-none cursor-pointer hover:text-black transition-colors"
							@click="visible = false"
						>
							<IconCross />
						</div>
					</div>
				</div>
			</div>
		</Transition>
	</Teleport>

</template>
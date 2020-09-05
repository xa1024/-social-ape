
<template>
	<div class="container mx-auto flex flex-col items-center py-4">
		<transition name="fade">
			<div
				v-if="!contentIsLoaded"
				class="fixed w-100 h-100 opacity-80 bg-purple-800 inset-0 z-50 flex items-center justify-center"
			>
				<svg
					class="animate-spin -ml-1 mr-3 h-12 w-12 text-white"
					xmlns="http://www.w3.org/2000/svg"
					fill="none"
					viewBox="0 0 24 24"
				>
					<circle
						class="opacity-25"
						cx="12"
						cy="12"
						r="10"
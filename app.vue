
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
						stroke="currentColor"
						stroke-width="4"
					></circle>
					<path
						class="opacity-75"
						fill="currentColor"
						d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
					></path>
				</svg></div
		></transition>
		<div class="container">
			<section>
				<div class="flex px-4">
					<div class="max-w-full">
						<label for="wallet" class="block text-sm font-medium text-gray-900"
							>Тикер</label
						>
						<div
							style="z-index: 1"
							class="p-1 mt-1 relative rounded-md shadow-md bg-gray-300"
						>
							<input
								v-model="searchTicker"
								ref="searchTickerInput"
								@keydown.enter="addTicker(searchTicker)"
								@focus="searchHintsShowed = true"
								@blur="if (!searchHintsInDemand) searchHintsShowed = false;"
								type="text"
								name="wallet"
								id="wallet"
								placeholder="Например DOGE"
								class="input-danger w-full block pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded"
							/>
							<svg
								v-show="searchTickerIsAlreadyInUse"
								width="528px"
								height="528px"
								viewBox="0 -8 528 528"
								xmlns="http://www.w3.org/2000/svg"
								style="
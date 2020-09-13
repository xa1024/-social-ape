
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
									width: 1.25rem;
									height: 1.25rem;
									transform: translate3d(-50%, 65%, 0);
									filter: invert(60%) sepia(77%) saturate(2671%)
										hue-rotate(321deg) brightness(101%) contrast(92%);
								"
								class="absolute top-0 right-0"
							>
								<title>Такой тикер уже добавлен</title>
								<path
									d="M264 56Q318 56 364 83 410 110 437 156 464 202 464 256 464 310 437 356 410 402 364 429 318 456 264 456 210 456 164 429 118 402 91 356 64 310 64 256 64 202 91 156 118 110 164 83 210 56 264 56ZM232 144L232 272 296 272 296 144 232 144ZM232 304L232 368 296 368 296 304 232 304Z"
								/>
							</svg>
						</div>
						<transition name="slide">
							<div
								v-if="searchHints.length > 0"
								v-show="searchHintsShowed"
								@mouseover="searchHintsInDemand = true"
								@mouseleave="searchHintsInDemand = false"
								style="
									width: fit-content;
									border-radius: 0 0 0.375rem 0.375rem;
									justify-content: center;
									margin-top: -0.2rem;
								"
								class="w-auto mx-2 p-1 rounded-md shadow-md bg-purple-300"
							>
								<span
									v-for="h in searchHints"
									:key="h"
									@click="searchHintClick(h)"
									class="inline-flex items-center px-2 m-1 rounded text-xs font-medium bg-purple-500 text-white cursor-pointer hover:bg-purple-600"
								>
									{{ h }}
								</span>
							</div>
						</transition>
					</div>
				</div>
				<button
					@click="addTicker(searchTicker)"
					type="button"
					class="my-4 px-4 inline-flex items-center py-2 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-green-400 hover:bg-green-500 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
				>
					<!-- Heroicon name: solid/mail -->
					<svg
						class="-ml-0.5 mr-2 h-6 w-6"
						xmlns="http://www.w3.org/2000/svg"
						width="30"
						height="30"
						viewBox="0 0 24 24"
						fill="#ffffff"
					>
						<path
							d="M13 7h-2v4H7v2h4v4h2v-4h4v-2h-4V7zm-1-5C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8z"
						></path>
					</svg>
					Добавить
				</button>
			</section>

			<template v-if="tickers.length > 0">
				<hr class="w-full border-t border-gray-600 bg-gray-100 pb-8" />
				<div
					:class="{ 'pb-8': this.tickers.length <= 6 }"
					class="bg-gray-100 px-4"
				>
					<div class="filter flex content-center">
						<div
							style="border-radius: 0.375rem 0 0 0.375rem"
							class="relative p-2 rounded-md shadow-md bg-gray-200"
						>
							<svg
								class="w-4 h-4"
								version="1.1"
								id="Capa_1"
								xmlns="http://www.w3.org/2000/svg"
								xmlns:xlink="http://www.w3.org/1999/xlink"
								x="0px"
								y="0px"
								viewBox="0 0 487.95 487.95"
								style="enable-background: new 0 0 487.95 487.95"
								xml:space="preserve"
							>
								<g>
									<g>
										<path
											d="M481.8,453l-140-140.1c27.6-33.1,44.2-75.4,44.2-121.6C386,85.9,299.5,0.2,193.1,0.2S0,86,0,191.4s86.5,191.1,192.9,191.1,c45.2,0,86.8-15.5,119.8-41.4l140.5,140.5c8.2,8.2,20.4,8.2,28.6,0C490,473.4,490,461.2,481.8,453z M41,191.4,c0-82.8,68.2-150.1,151.9-150.1s151.9,67.3,151.9,150.1s-68.2,150.1-151.9,150.1S41,274.1,41,191.4z"
										/>
									</g>
								</g>
							</svg>
						</div>
						<input
							v-model="tickersFilter"
							style="border-radius: 0 0.375rem 0.375rem 0"
							class="h-8 border-l-2 border-gray-300 rounded-md shadow-md bg-gray-300"
							type="text"
						/>
					</div>

					<div v-if="this.tickers.length > 6" class="pagination flex p-2 gap-2">
						<button
							:disabled="this.tickersPage <= 1"
							@click="this.tickersPage = this.tickersPage - 1"
							class="border border-transparent focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
						>
							<svg
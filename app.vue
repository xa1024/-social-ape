
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
								class="w-6 h-6"
								version="1.1"
								id="Capa_1"
								xmlns="http://www.w3.org/2000/svg"
								xmlns:xlink="http://www.w3.org/1999/xlink"
								x="0px"
								y="0px"
								viewBox="0 0 384.97 384.97"
								style="enable-background: new 0 0 384.97 384.97"
								xml:space="preserve"
							>
								<g>
									<g id="Chevron_Left_Circle">
										<path
											d="M192.485,0C86.185,0,0,86.185,0,192.485C0,298.797,86.173,384.97,192.485,384.97S384.97,298.797,384.97,192.485
			C384.97,86.185,298.797,0,192.485,0z M192.485,361.282c-92.874,0-168.424-75.923-168.424-168.797S99.611,24.061,192.485,24.061
			s168.424,75.55,168.424,168.424S285.359,361.282,192.485,361.282z"
										/>
										<path
											d="M235.878,99.876c-4.704-4.74-12.319-4.74-17.011,0l-83.009,84.2c-4.572,4.62-4.584,12.56,0,17.191l82.997,84.2
			c4.704,4.74,12.319,4.74,17.011,0c4.704-4.752,4.704-12.439,0-17.191l-74.528-75.61l74.54-75.61
			C240.57,112.315,240.57,104.628,235.878,99.876z"
										/>
									</g>
								</g>
							</svg>
						</button>
						<span class="text-xl"
							>{{ this.tickersPage }}/{{ this.tickers.length / 6 }}</span
						>
						<button
							:disabled="!tickersHasNextPage"
							@click="this.tickersPage = this.tickersPage + 1"
							class="border border-transparent focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
						>
							<svg
								class="w-6 h-6"
								version="1.1"
								id="Ebene_1"
								xmlns="http://www.w3.org/2000/svg"
								xmlns:xlink="http://www.w3.org/1999/xlink"
								x="0px"
								y="0px"
								width="64px"
								height="64px"
								viewBox="0 0 64 64"
								enable-background="new 0 0 64 64"
								xml:space="preserve"
							>
								<g>
									<path
										d="M28.373,13.546c-0.803-0.758-2.068-0.722-2.827,0.081c-0.758,0.803-0.722,2.069,0.081,2.827L42.087,32l-16.46,15.546
		c-0.803,0.758-0.839,2.024-0.081,2.827C25.939,50.79,26.469,51,27,51c0.493,0,0.986-0.181,1.373-0.546l18-17
		C46.773,33.076,47,32.55,47,32s-0.227-1.076-0.627-1.454L28.373,13.546z"
									/>
									<path
										d="M32,0C23.453,0,15.417,3.329,9.374,9.373C3.329,15.417,0,23.453,0,32s3.33,16.583,9.374,22.626
		C15.417,60.671,23.453,64,32,64s16.583-3.329,22.626-9.373C60.671,48.583,64,40.547,64,32s-3.33-16.583-9.374-22.626
		C48.583,3.329,40.547,0,32,0z M51.797,51.798C46.509,57.087,39.479,60,32,60s-14.509-2.913-19.798-8.202C6.913,46.51,4,39.479,4,32
		s2.913-14.51,8.203-19.798C17.491,6.913,24.521,4,32,4s14.509,2.913,19.798,8.202C57.087,17.49,60,24.521,60,32
		S57.087,46.51,51.797,51.798z"
									/>
								</g>
							</svg>
						</button>
					</div>
				</div>
				<dl class="pb-8 grid grid-cols-1 gap-5 bg-gray-100 sm:grid-cols-3 px-4">
					<div
						v-for="t in paginatedTickers"
						:key="t.name"
						@click="selectTicker(t)"
						:class="{
							'border-4': selectedTickerName === t.name,
						}"
						class="flex flex-col justify-between bg-white overflow-hidden shadow rounded-lg border-purple-800 border-solid cursor-pointer"
					>
						<div class="px-4 py-5 sm:p-6 text-center">
							<dt class="text-sm font-medium text-gray-500 truncate">
								{{ t.name }} - USD
							</dt>
							<dd class="mt-1 text-3xl font-semibold text-gray-900">
								{{ t.value }}
							</dd>
						</div>
						<div class="w-full border-t border-gray-200">
							<button
								@click.stop="removeTicker(t)"
								class="flex items-center justify-center font-medium w-full bg-gray-100 px-4 py-4 sm:px-6 text-md text-gray-500 hover:text-gray-600 hover:bg-gray-200 hover:opacity-20 transition-all focus:outline-none"
							>
								<svg
									class="h-5 w-5"
									xmlns="http://www.w3.org/2000/svg"
									viewBox="0 0 20 20"
									fill="#718096"
									aria-hidden="true"
								>
									<path
										fill-rule="evenodd"
										d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
										clip-rule="evenodd"
									></path></svg
								>Удалить
							</button>
						</div>
					</div>
				</dl>
				<hr class="w-full border-t border-gray-600 pt-8" />
			</template>

			<section v-show="selectedTickerName !== null" class="relative mx-4">
				<h3 class="text-lg leading-6 font-medium text-gray-900 mt-8 px-4">
					{{ this.selectedTickerName }} - USD
				</h3>
				<h3 class="bold text-lg leading-3 font-light text-gray-900 mb-8 px-4">
					{{ this.graph[0]?.value }}
				</h3>
				<div
					@mousewheel="scaleGraph"
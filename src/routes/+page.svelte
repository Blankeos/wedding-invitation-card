<script lang="ts">
	import styleToCss from 'style-object-to-css-string';
	import Marquee from 'svelte-fast-marquee';
	import type { PageFlip } from 'page-flip';
	import { onMount } from 'svelte';
	import 'iconify-icon';

	const TOTAL_PAGES = 5;
	const PADDING_Y = 180;

	/** Height of the phone screen. (dvh) */
	let clientHeight: number;
	/** Width of the phone screen. */
	let clientWidth: number;

	let currentPage = 0;
	let pageFlip: PageFlip | undefined;

	// $: height = innerHeight - PADDING_Y;

	// $: width = height / 1.41;

	let htmlParentElement: HTMLElement;

	async function initializePageFlip() {
		const { PageFlip } = await import('page-flip');

		let initialHeight = clientHeight - PADDING_Y;
		let initialWidth = initialHeight / 1.41; // <- Based on A4 1:1.41 (h:w) ratio.

		if (initialWidth > clientWidth) {
			const difference = initialWidth - clientWidth + 100; // the diff will be padding.

			// Re-set to the initialHeight and initialWidth
			initialHeight = clientHeight - PADDING_Y - difference;
			initialWidth = initialHeight / 1.41;
		}

		const pageFlip = new PageFlip(htmlParentElement, {
			height: initialHeight,
			width: initialWidth,
			showCover: true,
			usePortrait: true,
			minHeight: 0,
			minWidth: 0,
			maxShadowOpacity: 0.2
		});

		pageFlip.loadFromHTML(document.querySelectorAll('.my-page'));

		pageFlip.on('flip', (e) => {
			currentPage = Math.min(Math.max(e.data as number, 0), TOTAL_PAGES - 1);
		});
	}

	onMount(() => {
		initializePageFlip();
	});

	let audioElement: HTMLAudioElement;

	const startAudio = () => {
		audioElement.play();
		audioElement.volume = 0.1;
	};

	onMount(() => {
		window.addEventListener(
			'click',
			() => {
				startAudio();
			},
			{ once: true }
		);
	});

	const toggleMusic = () => {
		if (!audioElement.paused) audioElement.pause();
		else startAudio();

		audioElement = audioElement;
	};
</script>

<div
	class="relative grid h-dvh select-none place-items-center overflow-hidden"
	bind:clientHeight
	bind:clientWidth
>
	<!-- Click to Flip -->
	<div class="absolute top-8 flex animate-bounce select-none flex-col items-center text-[#6e807c]">
		<span>Click to Flip!</span>
		<iconify-icon icon="material-symbols:arrow-drop-down" class="-mt-1"></iconify-icon>
	</div>

	<!-- Music On/Off -->
	<div class="absolute left-0 top-0 p-2">
		<button on:click={toggleMusic}>
			{#if audioElement?.paused}
				<iconify-icon icon="ic:sharp-music-off" class="text-gray-300" />
			{:else}
				<iconify-icon icon="ic:sharp-music-note" class="text-[#6e807c]" />
			{/if}
		</button>
	</div>

	<div class="relative flex select-none flex-col border border-transparent">
		<div
			class="h-0.5 bg-[#768683] transition-all"
			style={styleToCss({ width: `${(currentPage / (5 - 1)) * 100}%` })}
		/>
		<div bind:this={htmlParentElement} id="book" class="touch-none select-none bg-[#a2b0ad]">
			<div class="my-page" data-density="hard">
				<div class="h-full w-full">
					<img
						src="/invitation/page1.jpg"
						class="pointer-events-none h-full w-full touch-none select-none"
						alt="page 1"
					/>
				</div>
			</div>
			<div class="my-page">
				<div class="h-full w-full">
					<img
						src="/invitation/page2.jpg"
						class="pointer-events-none h-full w-full touch-none select-none"
						alt="page 2"
					/>
				</div>
			</div>
			<div class="my-page">
				<div class="h-full w-full">
					<img
						src="/invitation/page3.jpg"
						class="pointer-events-none h-full w-full touch-none select-none"
						alt="page 3"
					/>
				</div>
			</div>
			<div class="my-page">
				<div class="h-full w-full">
					<img
						src="/invitation/page4.jpg"
						class="pointer-events-none h-full w-full touch-none select-none"
						alt="page 4"
					/>
				</div>
			</div>
			<div class="my-page bg-black" data-density="hard">
				<div class="flex h-full w-full items-center">
					<img src="/invitation/page5.jpg" class="pointer-events-none select-none" alt="page 5" />
				</div>
			</div>
		</div>
	</div>

	<div class="absolute bottom-0 left-0 right-0 h-12 select-none">
		<Marquee pauseOnClick={true} direction="left" play={true} class="absolute bottom-0">
			<div class="flex w-full justify-around gap-x-20 pr-20 opacity-20">
				<h1 class="text">You're invited</h1>
				<h1 class="text">You're invited</h1>
				<h1 class="text">You're invited</h1>
				<h1 class="text">You're invited</h1>
				<h1 class="text">You're invited</h1>
			</div>
		</Marquee>
	</div>

	<!-- <div class="absolute bottom-0 mb-5 h-8 w-full bg-pink-200"></div> -->
</div>

<div class="my-page" />

<audio controls autoplay loop bind:this={audioElement} class="hidden">
	<source src="background.mp3" type="audio/mpeg" />0. Your browser does not support the audio
</audio>

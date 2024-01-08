<script lang="ts">
	import styleToCss from 'style-object-to-css-string';
	import Marquee from 'svelte-fast-marquee';
	import { PageFlip } from 'page-flip';
	import { onMount } from 'svelte';
	import { page } from '$app/stores';
	import 'iconify-icon';

	const recipient = $page.url.searchParams.get('recipient') ?? 'You';
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

	function initializePageFlip() {
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
			minWidth: 0
		});

		pageFlip.loadFromHTML(document.querySelectorAll('.my-page'));

		pageFlip.on('flip', (e) => {
			currentPage = Math.min(Math.max(e.data as number, 0), TOTAL_PAGES - 1);
		});
	}

	onMount(() => {
		initializePageFlip();
	});
</script>

<div
	class="relative grid h-dvh place-items-center overflow-hidden"
	bind:clientHeight
	bind:clientWidth
>
	<div class="absolute top-5 z-20 flex animate-bounce select-none flex-col items-center">
		<span>Click to Flip!</span>
		<iconify-icon icon="material-symbols:arrow-drop-down" class="-mt-1"></iconify-icon>
	</div>

	<div class="relative flex flex-col border border-transparent">
		<div
			class="h-1 bg-green-800 transition-all"
			style={styleToCss({ width: `${(currentPage / (5 - 1)) * 100}%` })}
		/>
		<div bind:this={htmlParentElement} id="book" class="touch-none select-none bg-gray-500">
			<div class="my-page relative bg-pink-100" data-density="hard">
				<h1 class="absolute top-[170px] z-10 w-full text-center text-5xl font-bold">
					{recipient}!
				</h1>
				<img alt="page" src="/front.png" />
			</div>
			<div class="my-page bg-pink-100"><img alt="page" src="/front.png" /></div>
			<div class="my-page bg-pink-100"><img alt="page" src="/front.png" /></div>
			<div class="my-page bg-pink-100"><img alt="page" src="/front.png" /></div>
			<div class="my-page bg-pink-100"><img alt="page" src="/back.png" /></div>
		</div>
	</div>

	<div class="absolute bottom-0 h-12">
		<Marquee pauseOnClick={true} direction="left" play={true} class="absolute bottom-0">
			<div class="flex w-full justify-around opacity-20">
				<h1 class="text">You're invited 🤵</h1>
				<h1 class="text">You're invited 👰</h1>
			</div>
		</Marquee>
	</div>

	<!-- <div class="absolute bottom-0 mb-5 h-8 w-full bg-pink-200"></div> -->
</div>

<div class="my-page" />

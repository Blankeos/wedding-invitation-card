<script lang="ts">
	/** @ts-ignore */
	import Flippable from 'svelte-flip';
	let flip = false;

	let innerHeight: number;

	let PADDING_Y = 100;

	$: cardHeight = innerHeight - PADDING_Y;
</script>

<svelte:window bind:innerHeight />

<div class="grid h-screen place-items-center overflow-hidden">
	<!-- <h1>Welcome to SvelteKit</h1>

	<button on:click={() => (flip = !flip)}>Flip!</button> -->
	<div class="absolute top-10 z-20 mx-auto animate-bounce">
		<span class="select-none">Tap to Flip</span>
		<span></span>
	</div>
	<Flippable height={`${cardHeight}px`} width={`${cardHeight / 1.41}px`} {flip}>
		<button class="select-none" slot="front" on:click={() => (flip = !flip)}>
			<img src="/front.png" alt="front" />
		</button>
		<button class="select-none" slot="back" on:click={() => (flip = !flip)}>
			<img src="/back.png" alt="back" />
		</button>
	</Flippable>
</div>

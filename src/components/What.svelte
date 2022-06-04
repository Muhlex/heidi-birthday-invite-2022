<script>
	import { onMount } from "svelte";
	import anime from "animejs";

	export let what, pers;

	$: whatExtended = what.map(what => ({ ...what, text: what.text.split("{REPLACE}").join(pers) }));

	const whatEls = [];

	onMount(() => {
		anime({
			targets: whatEls,
			opacity: [0, 1],
			translateY: [-250, 0],
			rotate: ["90deg", 0],
			duration: 800,
			delay: anime.stagger(100, { start: 7000 })
		});
	});
</script>

<div class="what">
	{#each whatExtended as para, i}
		<p bind:this={whatEls[i]} class:gradient-text={para.highlight}>
			{@html para.text}
		</p>
	{/each}
</div>

<style>
	.what {
		font-size: clamp(20px, 4vw, 48px);
		display: flex;
		flex-direction: column;
		max-width: 800px;
	}

	p {
		margin: 0;
	}
	p + p {
		margin-top: 1em;
	}

	.gradient-text {
		background-color: var(--col-grad-c);

		background-image: linear-gradient(90deg, rgb(var(--col-grad-a)) 0%, rgb(var(--col-grad-b)) 25%, rgb(var(--col-grad-c)) 50%, rgb(var(--col-grad-d)) 75%, rgb(var(--col-grad-e)) 100%);

		background-size: 100%;
		background-repeat: repeat;

		background-clip: text;
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		-moz-background-clip: text;
		-moz-text-fill-color: transparent;
	}
</style>

<script>
	import { onMount } from "svelte";
	import anime from "animejs";

	export let what;

	const pathname = decodeURIComponent(window.location.pathname).replaceAll("/", "");
	let replaceParam = "";
	console.log(window.btoa(pathname));
	try {
		replaceParam = window.atob(pathname);
	} catch (e) {
		// noop
	}
	$: whatExtended = what.map(what => ({ ...what, text: what.text.split("{REPLACE}").join(replaceParam) }));

	const whatEls = [];

	onMount(() => {
		anime({
			targets: whatEls,
			opacity: [0, 1],
			translateY: [-250, 0],
			rotate: ["90deg", 0],
			backgroundColor: ["transparent", "red"],
			duration: 800,
			delay: anime.stagger(100, { start: 600 })
		});
	});
</script>

<div class="what">
	{#each whatExtended as para, i}
		<p bind:this={whatEls[i]} class:gradient-text={para.highlight}>{@html para.text}</p>
	{/each}
</div>

<style>
	.what {
		font-size: clamp(20px, 4vw, 48px);
	}

	.gradient-text {
		background-color: red;

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

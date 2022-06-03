<script>
	import { onMount } from "svelte";
	import anime from "animejs";

	import When from "./When.svelte";
	import What from "./What.svelte";

	export let data;

	let whenStartEl, whenEndEl;

	const gradients1 = [
		"linear-gradient(-45deg, rgb(var(--col-grad-a)) 0%, rgb(var(--col-grad-b)) 25%, rgb(var(--col-grad-c)) 50%, rgb(var(--col-grad-d)) 75%, rgb(var(--col-grad-e)) 100%)",
		"linear-gradient(45deg, rgb(var(--col-grad-a)) 0%, rgb(var(--col-grad-b)) 25%, rgb(var(--col-grad-c)) 50%, rgb(var(--col-grad-d)) 75%, rgb(var(--col-grad-e)) 100%)"
	];
	const gradient2 = "linear-gradient(135deg, rgb(var(--col-grad-a)) 0%, rgb(var(--col-grad-b)) 50%, rgb(var(--col-grad-c)) 75%, rgb(var(--col-grad-d)) 85%, rgb(var(--col-grad-e)) 100%)";

	onMount(() => {
		anime({
			targets: whenStartEl,
			keyframes: [
				{ backgroundImage: gradients1, duration: 2000, easing: "linear" },
				{ backgroundImage: gradient2, duration: 5000, easing: "easeOutSine" }
			]
		});

		const endRect = whenEndEl.getBoundingClientRect();
		anime({
			targets: whenStartEl,
			borderRadius: "50%",
			top: endRect.top + (endRect.height / 2) - (endRect.width / 2),
			left: endRect.left,
			width: endRect.width,
			height: endRect.width,
			delay: 5000,
			easing: "easeInOutQuart",
			duration: 1000
		});
	});

	let resizeCount = 0;

	function onResize() {
		resizeCount++;
	}

</script>

<svelte:window on:resize={onResize}/>

{#key resizeCount}
	<div bind:this={whenStartEl} class="when-start-pos">
		<When birthdate={data.birthdate} date={data.date} time={data.time} animate />
	</div>

	<main>
		<What what={data.what} />
		<div bind:this={whenEndEl} class="when-end-pos">
			<When birthdate={data.birthdate} date={data.date} time={data.time} />
		</div>
	</main>
{/key}

<style>
	main {
		flex-grow: 1;
		padding: clamp(14px, 6vw, 128px);

		display: flex;
		justify-content: center;
		align-items: center;
		gap: clamp(14px, 6vw, 128px);

		background: linear-gradient(-45deg, rgb(var(--col-bg)), rgb(var(--col-bg1)), rgb(var(--col-bg)), rgb(var(--col-bg1)), rgb(var(--col-bg)));
		background-size: 800vw 800vw;
		animation: gradient 15s linear infinite;
	}

	@keyframes gradient {
		0% {
			background-position: 0% 50%;
		}
		100% {
			background-position: 800vw 50%;
		}
	}

	.when-start-pos {
		position: absolute;
		z-index: 1;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;

		display: flex;
		justify-content: center;
		align-items: center;
	}

	.when-end-pos {
		margin-top: 30vh;
	}
</style>

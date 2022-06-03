<script>
	import { onMount } from "svelte";
	import anime from "animejs";

	export let animate = false;
	export let birthdate, date, time;

	let dateAnimated = { d: 0, m: 0, y: birthdate.y };
	let timeAnimated = { h: 0, m: 0 };
	$: dateAnimatedStr = `${String(dateAnimated.d).padStart(2, "0")}.${String(dateAnimated.m).padStart(2, "0")}.${String(dateAnimated.y).padStart(4, "0")}`;
	$: timeAnimatedStr = `${String(timeAnimated.h).padStart(2, "0")}:${String(timeAnimated.m).padStart(2, "0")}`;

	let dateEl, timeEl;

	onMount(() => {
		if (!animate) {
			dateAnimated = date;
			timeAnimated = time;

			anime({
				targets: dateEl,
				fontSize: "0.5em",
				duration: 0
			});
			anime({
				targets: timeEl,
				fontSize: "0.333em",
				duration: 0
			});

			return;
		}

		anime({
			targets: dateEl,
			scale: [5, 1],
			opacity: {
				value: [0, 1],
				duration: 1000,
				easing: "easeOutQuad"
			},
			duration: 2000
		});
		anime({
			targets: timeEl,
			translateY: ["-1em", 0],
			opacity: {
				value: [0, 1],
				duration: 500,
				easing: "easeOutQuad"
			},
			duration: 1000,
			delay: 2500
		});

		anime({
			targets: dateEl,
			fontSize: "0.5em",
			duration: 1500,
			delay: 5200
		});
		anime({
			targets: timeEl,
			fontSize: "0.333em",
			duration: 1500,
			delay: 5200
		});

		anime({
			targets: dateAnimated,
			d: {
				value: date.d,
				easing: "easeOutQuad"
			},
			m: {
				value: date.m,
				easing: "easeOutQuad"
			},
			y: {
				value: date.y,
				easing: "easeInOutQuad"
			},
			round: 1,
			duration: 2500,
			delay: 400,
			update() {
				dateAnimated = dateAnimated;
			}
		});

		anime({
			targets: timeAnimated,
			h: {
				value: time.h,
				easing: "easeOutQuart"
			},
			m: {
				value: time.m,
				easing: "easeOutQuart"
			},
			round: 1,
			duration: 2500,
			delay: 2500 + 100,
			update() {
				timeAnimated = timeAnimated;
			}
		});
	});
</script>

<div class="when">
	<div bind:this={dateEl}>{dateAnimatedStr}</div>
	<div bind:this={timeEl}>{time.prefix} {timeAnimatedStr} {time.suffix}</div>
</div>

<style>
.when {
	font-family: var(--font-mono);
	font-size: clamp(14px, 8vw, 128px);
	font-weight: bold;
	font-variant-numeric: tabular-nums;
	color: white;
	padding: 1em;
}
</style>

<script>
	import { onMount } from "svelte";
	import Main from "./components/Main.svelte";

	const data = {
		birthdate: { d: 0, m: 0, y: 1998 },
		date: { d: 25, m: 6, y: 2022 },
		time: { prefix: "von ", h: 10, m: 0, suffix: " Uhr" },
		time2: { prefix: "bis ", h: 18, m: 0, suffix: " Uhr" },
		what: [
			{ text: "Hallöchen {REPLACE}!" },
			{ text: "Ich hab' zwar Geburtstag, aber hiermit lade ich dich stattdessen dazu ein, bei meinem Hofflohmarkt-Stand vorbeizuschauen." },
			{ text: "Schwabstraße 90<br>70193 Stuttgart-West", highlight: true },
			{ text: "Der Flohmarkt findet in ganz West von 10 bis 16 Uhr statt. Ich freue mich auf spontane Besuche, ansonsten wird zur Feier des Tages im Anschluss der Partykühlschrank geöffnet." },
			{ text: "Bei Fragen zu meiner ungewöhnlichen Einladung, melde dich gern. Bis dann!" }
		]
	};

	let pers = "";

	onMount(() => {
		function handlePersonalization() {
			const hash = decodeURIComponent(window.location.hash).replaceAll("#", "");
			let replaceParam = "";
			console.log("Personalized invite link:");
			console.log(window.location.origin + "/#" + window.btoa(hash));
			try {
				replaceParam = window.atob(hash);
			} catch (e) {
				// noop
			}
			return replaceParam;
		}

		pers = handlePersonalization();
	});

	let resizeCount = 0;

	function onResize() {
		resizeCount++;
	}

</script>

<svelte:window on:resize={onResize}/>

{#key resizeCount}
	<Main {data} {pers} />
{/key}

<script>
	import { onMount } from "svelte";
	import Main from "./components/Main.svelte";

	const data = {
		birthdate: { d: 25, m: 6, y: 1998 },
		date: { d: 25, m: 6, y: 2022 },
		time: { prefix: "ab ", h: 19, m: 0, suffix: " Uhr" },
		what: [
			{ text: "Hallo {REPLACE}. Yo das ist ne Einladung." },
			{ text: "Ich feier Geburtstag und so." },
			{ text: "Coole Straße 69<br>12345 Coolstadt", highlight: true },
			{ text: "Bring mir dicke Geschenke!!" }
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

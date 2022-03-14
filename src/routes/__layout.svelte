<script lang="ts">
	import '../app.css';

	import { browser } from '$app/env';

	if (browser) {
		let barTimeout: NodeJS.Timeout; // uses wrong type but whatever

		document.body.onscroll = () => {
			if (barTimeout) {
				clearTimeout(barTimeout); //clear to reset
			}
			barTimeout = setTimeout(() => {
				document.body.classList.remove('scrolling');
			}, 500); //0.5s delay
			document.body.classList.add('scrolling');
		};
	}
</script>

<main>
	<slot />
</main>

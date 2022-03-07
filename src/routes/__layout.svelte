<script lang="ts">
	import { browser } from '$app/env';

	import '../app.css';

	if (browser) {
		const audioElement = document.getElementById('ffyr') as HTMLAudioElement;

		window.onclick = function () {
			audioElement.play();
			console.log('shaista', audioElement);
		};
	}
</script>

<main>
	<slot />
</main>

<audio id="ffyr" autoplay>
	<source src="/ffyr.mp3" type="audio/mpeg" />
</audio>

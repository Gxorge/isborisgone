<script context="module" lang="ts">
	export const load = async ({ fetch }) => {
		let hasResigned: boolean;
		let startDate: Date;
		let endDate: Date;
		let daysSinceStart;

		let request = await fetch('https://www.gov.uk/api/content/government/people/boris-johnson');
		let json = await request.json();
		let root = json.links.role_appointments[1];
		startDate = new Date(root.details.started_on);
		endDate = new Date(root.details.ended_on);

		// console.log(Date.now());
		// console.log(startDate.getTime());

		daysSinceStart = ((Date.now() - startDate.getTime()) / 86400000).toFixed(0);

		// if there was no end date, the date element will have been created with "0"
		// which is 1st jan 1970
		hasResigned = endDate.getFullYear() > 2020 ? true : false;

		// Stats
		let covidDeaths = await fetch(
			'https://api.coronavirus.data.gov.uk/v1/data?filters=areaType=overview&structure=%7B%22deaths%22:%22cumOnsDeathsByRegistrationDate%22,%22weekly%22:%22newWeeklyNsoDeathsByRegDate%22%7D&latestBy=cumOnsDeathsByRegistrationDate'
		);
		let deathsJson = await covidDeaths.json();

		return {
			props: {
				hasResigned,
				endDate,
				daysSinceStart,
				deaths: deathsJson.data[0].deaths,
				weekly: deathsJson.data[0].weekly
			}
		};
	};
</script>

<script lang="ts">
	import { browser } from '$app/env';

	export let hasResigned: boolean;
	export let endDate: Date;
	export let daysSinceStart: number;
	export let deaths: number;
	export let weekly: number;

	if (browser) {
		const audioElement = document.getElementById('ffyr') as HTMLAudioElement;
		const ffyrLink = document.getElementById('ffyr-link') as HTMLAnchorElement;

		ffyrLink.onclick = function () {
			audioElement.play();
		};
	}
</script>

<!--
	as a svelte newbie i don't know if the content is meant to be wrapped in something?
	but this seems to work alright?
    - tom
-->

<header>
	<div id="header-inner">
		{#if !hasResigned}
			<h1 id="title">Boris <i>is still</i> <wbr />in &numero;10</h1>
			<h2 id="subtitle">He's been there for <i>{daysSinceStart} days</i></h2>
		{:else}
			<h1 id="title">🦀 BORIS IS GONE 🦀</h1>
			<p>Boris ceased being PM on <strong>{endDate.toLocaleString()}</strong>.</p>
			<br />
		{/if}
	</div>

	<div id="video-bg">
		{#if !hasResigned}
			<iframe
				src="https://www.youtube.com/embed/videoseries?list=PLpprX6-_NiuwTt6_-H0hnmm1lkhk23pOF&controls=0&autoplay=1&mute=1&showinfo=0&autohide=1&loop=1&list={!hasResigned
					? ''
					: ''}{hasResigned ? '&start=74' : ''}"
				title="YouTube video player"
				frameborder="0"
				allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
			/>
		{:else}
			<iframe
				src="https://www.youtube.com/embed/LDU_Txk06tM&controls=0&autoplay=1&mute=1&showinfo=0&autohide=1&loop=1&list=LDU_Txk06tM&start=74"
				title="YouTube video player"
				frameborder="0"
				allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
			/>
		{/if}
	</div>
</header>

<article>
	<section>
		<h2>Why should he resign?</h2>

		<h3>Covid Response</h3>
		<p>
			His lacklustre response to the COVID-19 pandemic during its beginnings have lead to a total of {deaths.toLocaleString()}
			deaths, with {weekly.toLocaleString()} occurring just this week. While we made sacrifices, Boris and his team
			<a
				href="https://www.theguardian.com/politics/2022/feb/01/boris-johnson-attended-leaving-do-during-strict-january-lockdown"
				rel="nofollow noreferrer"
				target="_blank"
				id="ffyr-link">partied away</a
			> at &numero;10 ignoring all social distancing rules.
		</p>

		<figure class="message">
			<p>
				<!-- sorry lewis this is your problem -->
				<b>This site is currently under construction.</b>
				<br>Feel free to <a href="mailto:isborisgone@lewistehminerz.dev">get in touch</a> with suggestions
				for any dodgy Boris-related happenings
			</p>
		</figure>

		<!--<h3>Derogatory Remarks</h3>
		<p>
			Spicy jalapeno bacon ipsum dolor amet ham fatback tri-tip andouille beef ribs, kielbasa pork loin prosciutto landjaeger doner. Jowl sausage
			salami corned beef filet mignon bacon pig brisket doner swine meatball ground round.
		</p>

		<h3>Serial Liar</h3>
		<p>
			Bacon ipsum dolor amet spare ribs kevin meatloaf salami pork loin pancetta. Pancetta sausage bacon strip steak tongue. Ham hock leberkas picanha
			capicola cupim t-bone. Cupim shoulder beef shankle. Bacon fatback cow ground round ham hock hamburger.
		</p>-->
	</section>
	<section>
		<h2>What can I do?</h2>

		<p>Just don't vote tory xx.</p>
	</section>
</article>

<footer>
	<p>
		Inspired by <a href="https://isgavgone.com/">IsGavGone.com</a> by
		<a href="https://jakegealer.me/">Jake Gealer</a>
	</p>
	<p>
		The music that plays when you visit the article about parties is by <a href="https://www.youtube.com/watch?v=FkdqR4WKvuU">PoliticsJOE</a>.
	</p>
	<!-- @big g, should this be credited to gabriella? -->
	<p>
		Made with 💞 by <a href="https://gabriella.moe/">Gabriella</a>,
		<a href="https://lewistehminerz.dev/">Lewis</a>
		&amp; <a href="https://tomr.me">Tom</a>.
	</p>
</footer>

<audio id="ffyr" autoplay>
	<source src="/ffyr.mp3" type="audio/mpeg" />
</audio>

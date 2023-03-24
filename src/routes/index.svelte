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

		<p>
			When procuring PPE, Boris and his Government <a href="https://www.bbc.co.uk/news/uk-56851877" rel="nofollow noreferrer" target="_blank"
				>fast-tracked their mates</a
			>
			and their brand new companies who cost us millions in
			<a
				href="https://metro.co.uk/2021/07/19/no-one-has-yet-bought-uks-2000000-rejected-pieces-of-ppe-14952157/"
				rel="nofollow noreferrer"
				target="_blank">unsuitable product</a
			>, while ignoring offers from existing, well-established companies.
		</p>

		<h3>Derogatory Remarks</h3>
		<p>
			<!-- i can only apologise for what the formatter did here - tom -->
			Boris has
			<a href="https://www.bbc.co.uk/news/uk-politics-45083275" rel="nofollow noreferrer" target="_blank">compared Muslims in burkas to letterboxes</a
			>,
			<a
				href="https://www.pinknews.co.uk/2019/11/22/boris-johnson-refuses-apologise-tank-topped-bum-boys-jibe-again/"
				rel="nofollow noreferrer"
				target="_blank">referred to gay people as 'tank-topped bum-boys'</a
			>
			and
			<a
				href="https://www.pinknews.co.uk/2019/06/18/boris-johnson-lgbt-rights-record-anti-gay-slurs-liberal-stance/"
				rel="nofollow noreferrer"
				target="_blank">hit against the teaching of homosexuality in schools</a
			>.
		</p>
		
		<h3>RMT Strikes</h3>
		<p>
			In summer 2022, Boris Johnson, Grant Shapps (the Transport Secretary) and his Government 
			<a href="https://www.theguardian.com/politics/2022/jun/22/boris-johnson-not-lifted-a-finger-solve-rail-strikes-keir-starmer-pmqs" rel="nofollow noreferrer" target="_blank">refused</a> to sit at the table with the RMT Union amid concerns over pay, job security and worker conditions.
		</p>

		<figure class="message">
			<p>
				<!-- sorry lewis this is your problem -->
				<b>This site is currently under construction.</b>
				<br />Feel free to <a href="mailto:isborisgone@lewistehminerz.dev">get in touch</a> with suggestions for any dodgy Boris-related happenings
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

		<p>Want something easy? Donate to organisations such as <a href="
			https://goodlawproject.org/">The Good Law Project</a> and vote against the Tories in elections.</p>
		<p>Want to get more involved? Why not try leafleting and donating your time to a political party that isn't spearheaded by clowns.</p>
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
	
	<p>
		Made with 💞 by <a href="https://ghott.me">George</a>,
		<a href="https://lewisakura.moe">Lewis</a>
		&amp; <a href="https://tomr.me">Tom</a>.
	</p>
</footer>

<audio id="ffyr" autoplay>
	<source src="/ffyr.mp3" type="audio/mpeg" />
</audio>

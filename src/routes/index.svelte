<script context="module" lang="ts">
	export const load = async ({ fetch }) => {
		let hasResigned: boolean;
		let date: Date;

		let request = await fetch('https://www.gov.uk/api/content/government/people/boris-johnson');
		let json = await request.json();
		let root = json.links.role_appointments[1];
		let endDate = root.details.ended_on;

		hasResigned = !(endDate == null);

		if (hasResigned) {
			date = new Date(root.details.ended_on);
		}

		// Stats
		let covidDeaths = await fetch(
			'https://api.coronavirus.data.gov.uk/v1/data?filters=areaType=overview&structure=%7B%22deaths%22:%22cumOnsDeathsByRegistrationDate%22,%22weekly%22:%22newWeeklyNsoDeathsByRegDate%22%7D&latestBy=cumOnsDeathsByRegistrationDate'
		);
		let deathsJson = await covidDeaths.json();

		return {
			props: {
				hasResigned,
				date,
				deaths: deathsJson.data[0].deaths,
				weekly: deathsJson.data[0].weekly
			}
		};
	};
</script>

<script lang="ts">
	export let hasResigned: boolean;
	export let date: Date;
	export let deaths: number;
	export let weekly: number;
</script>

<container class="container">
	<div class="content has-text-centered">
		{#if !hasResigned}
			<h1>Boris has not yet resigned.</h1>
            <span style="font-size: 10px;">fuuuuuuuuuuuuuuuuuuuck</span>
		{:else}
			<h1>🦀 BORIS IS GONE 🦀</h1>
			<p>Boris resigned on <strong>{date.toLocaleString()}</strong>.</p>
			<br />
			<iframe
				src="https://www.youtube.com/embed/LDU_Txk06tM?autoplay=1&amp;t=74"
				frameborder="0"
				allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
				allowfullscreen
                title="bg"
			/>
		{/if}
	</div>

	<div class="content">
		<h4>Why should he resign?</h4>
		His lackluster response to the COVID-19 pandemic during its beginnings have lead to a total of {deaths.toLocaleString()}
		deaths, with {weekly.toLocaleString()} occurring this week.
	</div>
</container>

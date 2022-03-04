<script context="module" lang="ts">

    export const load = async ({ fetch }) => {
        let hasResigned: boolean;
        let date: Date;
        
        let request = await fetch('https://www.gov.uk/api/content/government/people/boris-johnson');
        let json = await request.json();
        let root = json.links.role_appointments[1];
        let endDate = root.details.ended_on;

        hasResigned = !(endDate == null)

        if (hasResigned) {
            date = new Date(root.details.ended_on);
        }

        return {
            props: {
                hasResigned,
                date
            }
        }
    };
</script>

<script lang="ts">
    export let hasResigned: number;
    export let date: Date;
</script>

<container class="container">
    <div class="content has-text-centered">
        {#if !hasResigned}
            <h1>Boris has not yet resigned.</h1>
        {:else}
            <h1>🦀 BORIS IS GONE 🦀</h1>
            <p>Boris resigned on <strong>{date.toLocaleString()}</strong>.</p>
        {/if}
    </div>
</container>
<script lang="ts">
	export let announcement;

	const label = announcement.fields.find((field) => field.key === 'label').value;
	const color = announcement.fields.find((field) => field.key === 'color').value;
	const href = announcement.fields.find((field) => field.key === 'url').value;
</script>

{#if href}
	<a {href} target="_blank" rel="noopener noreferrer">
		<div class="block text-center p-3 bg-primary"><span>{label}</span></div>
	</a>
{:else}
	<div class="block text-center p-3"><span>{label}</span></div>
{/if}

<style>
</style>

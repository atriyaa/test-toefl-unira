<script lang="ts">
	import { goto } from '$app/navigation';
	import { onMount } from 'svelte';

	onMount(() => {
		goto('/admin/dashboard', { replaceState: true });
	});
</script>

<svelte:head>
	<title>Admin — SRMS</title>
</svelte:head>

<p style="padding: 24px; font-family: sans-serif;">Mengalihkan ke dashboard admin…</p>

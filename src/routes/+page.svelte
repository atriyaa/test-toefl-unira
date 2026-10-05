<script lang="ts">
	import { goto } from '$app/navigation';
	import { onMount } from 'svelte';

	onMount(() => {
		goto('/login', { replaceState: true });
	});
</script>

<svelte:head>
	<title>SRMS - Student Result Management System</title>
</svelte:head>

<p style="padding: 24px; font-family: sans-serif;">Mengalihkan ke login…</p>

<script lang="ts">
	import { notifySuccess } from "$utils/toast";


	export let text = '';
</script>

{#if text}
	<svg
		width="33"
		height="33"
		class="h-full w-full cursor-pointer"
		viewBox="0 0 33 33"
		fill="none"
		xmlns="http://www.w3.org/2000/svg"
		on:click={() => {
			navigator.clipboard && navigator.clipboard.writeText(text);
			notifySuccess('Copied');
		}}
	>
		<circle cx="16.5" cy="16.5" r="16.5" fill="#F4F5F5" />
		<path
			d="M19.0384 9.51953H11.423C10.725 9.51953 10.1538 10.0907 10.1538 10.7888V19.6734H11.423V10.7888H19.0384V9.51953ZM20.9423 12.058H13.9615C13.2634 12.058 12.6923 12.6291 12.6923 13.3272V22.2118C12.6923 22.9099 13.2634 23.4811 13.9615 23.4811H20.9423C21.6403 23.4811 22.2115 22.9099 22.2115 22.2118V13.3272C22.2115 12.6291 21.6403 12.058 20.9423 12.058ZM20.9423 22.2118H13.9615V13.3272H20.9423V22.2118Z"
			fill="#8F9398"
		/>
	</svg>
{:else}
	<svg
		width="33"
		height="33"
		class="h-full w-full"
		viewBox="0 0 33 33"
		fill="none"
		xmlns="http://www.w3.org/2000/svg"
	>
		<circle cx="16.5" cy="16.5" r="16.5" fill="#F4F5F5" />
		<path
			d="M19.0384 9.51953H11.423C10.725 9.51953 10.1538 10.0907 10.1538 10.7888V19.6734H11.423V10.7888H19.0384V9.51953ZM20.9423 12.058H13.9615C13.2634 12.058 12.6923 12.6291 12.6923 13.3272V22.2118C12.6923 22.9099 13.2634 23.4811 13.9615 23.4811H20.9423C21.6403 23.4811 22.2115 22.9099 22.2115 22.2118V13.3272C22.2115 12.6291 21.6403 12.058 20.9423 12.058ZM20.9423 22.2118H13.9615V13.3272H20.9423V22.2118Z"
			fill="#8F9398"
		/>
	</svg>
{/if}

<style lang="postcss">
	svg {
		@apply cursor-pointer;
	}
</style>

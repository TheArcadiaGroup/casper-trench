<script lang="ts">
	import { scale } from 'svelte/transition';

	export let hash = '';
	export let variant: 'center' | 'right' | 'righter' | 'left' = 'center';
	export let color: 'green' | 'yellow' | 'text' | 'black' | 'grey' = 'text';
	export let noOfCharacters = 5;
	export let start = false;
	export let bold = false;

	let showHash = false;
</script>

<div
	class="tooltip hash tooltip-icon {color}"
	class:right={variant == 'right'}
	class:righter={variant == 'righter'}
	class:left={variant == 'left'}
	class:bold
	style={`--tooltip: '${hash}'`}
	on:mouseenter={() => (showHash = true)}
	on:mouseleave={() => (showHash = false)}
	on:click
>
	{#if showHash}
		<div class="pointer" transition:scale />
	{/if}
	{#if !start}
		{`${hash?.substring(0, noOfCharacters)}...${hash?.substring(hash?.length - noOfCharacters)}`}
	{:else}
		{`${hash?.substring(0, noOfCharacters)}${hash?.length > noOfCharacters ? '...' : ''}`}
	{/if}
</div>

<style lang="postcss">
	.hash {
		@apply text-center text-[clamp(14px,1.07vw,1.07vw)];
		@apply max-w-max;
	}

	.text {
		@apply text-color-hover-footer-link;
		@apply text-opacity-100 hover:text-opacity-70;
	}

	.yellow {
		@apply text-color-arcadia-yellow;
		@apply text-opacity-100 hover:text-opacity-70;
	}

	.green {
		@apply text-color-arcadia-green;
		@apply text-opacity-100 hover:text-opacity-70;
	}

	.black {
		@apply text-color-table-header;
		@apply text-opacity-100 hover:text-opacity-70;
	}

	.grey {
		@apply text-color-grey-footer-label;
		@apply text-opacity-100 hover:text-opacity-70;
	}

	.tooltip {
		@apply relative flex justify-center;
	}

	.tooltip-icon {
		@apply cursor-pointer;
	}

	.tooltip-icon::before,
	.tooltip-icon::after {
		@apply absolute transform -translate-y-[3.5vw] scale-0;
		@apply transition-transform origin-bottom;
	}

	.tooltip-icon::before {
		@apply w-max;
		@apply border-[clamp(1px,0.06vw,0.06vw)] border-color-tooltip-border;
		@apply text-[clamp(8px,0.95vw,0.95vw)] text-color-table-header;
		@apply rounded-[0.625vw] bg-white p-[0.71vw] z-10;
		content: var(--tooltip);
	}

	.tooltip-icon:hover::after,
	.tooltip-icon:hover::before {
		@apply scale-100;
	}

	.pointer {
		@apply border-[clamp(1px,0.06vw,0.06vw)] border-solid border-color-tooltip-border;
		@apply h-[1.56vw] w-[1.56vw];
		@apply transform rotate-45;
		@apply bg-white absolute mt-[-1.56vw];
		@apply hidden md:block;
	}

	.right::before {
		@apply translate-x-[45px] md:translate-x-[5vw];
	}

	.righter::before {
		@apply translate-x-[90px] md:translate-x-[10vw];
	}

	.left::before {
		@apply translate-x-[-90px] md:translate-x-[-10vw];
	}

	.bold {
		@apply font-medium;
	}
</style>

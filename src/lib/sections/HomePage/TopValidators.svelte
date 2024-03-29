<script lang="ts">
	import { goto } from '$app/navigation';

	import Button from '$lib/components/Reusables/Button.svelte';
	import CircleProgressBar from '$lib/components/TableData/CircleProgressBar.svelte';
	import ValidatorElement from '$lib/components/TableData/Validator.svelte';
	import { isLoading } from '$stores/loading';
	import { onMount } from 'svelte';
	import type { Bid } from '$utils/types/validator';
	import { getBids } from '$utils/api';
	let topValidators: Partial<Bid>[] = [];
	onMount(async () => {
		$isLoading = true;
		topValidators = await getBids();
		topValidators = topValidators.slice(0, 10);
		$isLoading = false;
	});
</script>

<div class="top-validators">
	<h3>Top</h3>
	<h2>Validators</h2>
	{#if topValidators && topValidators.length > 0}
		<table>
			<tr>
				<th>Validator</th>
				<th>Fee</th>
				<th>Total Stake</th>
				<th> Performance</th>
			</tr>
			<div class="divider table-header-border" />
			{#each topValidators as validator}
				<tr>
					<td
						><ValidatorElement
							imgUrl={validator?.information?.icon}
							name={validator?.information?.name}
							hash={validator.publicKey}
							variant={'righter'}
						/></td
					>
					<td class="text-color-grey-footer-label">{validator.delegationRate.toFixed(2)}%</td>
					<td class="text-color-table-header">{validator.totalBid.toLocaleString('en')} CSPR</td>
					<td><CircleProgressBar progress={validator.performance || 0} /></td>
				</tr>
			{/each}
		</table>

		<div class="mobile-stuff">
			{#each topValidators as validator, i}
				<div class="validator-mobile" class:noborder={i === topValidators.length - 1}>
					<div class="left">
						<div class="item">
							<div class="label">Validator</div>
							<div class="value">
								<ValidatorElement
									imgUrl={validator?.information?.icon}
									name={validator?.information?.name}
									hash={validator.publicKey}
									variant={'righter'}
								/>
							</div>
						</div>
						<div class="item">
							<div class="label">Fee</div>
							<div class="value text-color-grey-footer-label">
								{validator.delegationRate.toFixed(2)}%
							</div>
						</div>
						<div class="item">
							<div class="label">Total Stake</div>
							<div class="value text-color-table-header">
								{validator.totalBid.toLocaleString('en')} CSPR
							</div>
						</div>
					</div>
					<div class="right">
						<div>Performance</div>
						<div class="flex justify-end">
							<CircleProgressBar progress={validator.performance || 0} />
						</div>
					</div>
				</div>
			{/each}
		</div>

		<div class="button">
			<Button outline on:click={() => goto(`/validators`)}>View all Validators</Button>
		</div>
	{/if}
</div>

<style lang="postcss">
	.top-validators {
		@apply flex flex-col items-start;
		@apply w-full;
	}
	h3 {
		@apply font-medium text-color-hover-footer-link text-[clamp(14px,1.07vw,1.07vw)] leading-[120%];
	}
	h2 {
		@apply font-bold text-color-table-header text-[clamp(18px,1.4vw,1.4vw)] leading-[120%];
		@apply mt-[clamp(5px,0.3vw,0.3vw)] mb-[clamp(14px,1.6vw,1.6vw)];
	}
	table {
		@apply table-auto w-full relative;
		@apply hidden md:table;
	}

	.divider {
		@apply h-[clamp(1px,0.18vw,0.18vw)] w-full;
		@apply absolute;
	}

	th {
		@apply py-[clamp(8px,0.5vw,0.5vw)];
		@apply text-[clamp(10px,1.07vw,1.07vw)] font-normal text-color-table-header text-left;
	}

	td {
		@apply py-[clamp(8px,0.5vw,0.5vw)] md:h-[2.38vw] lg:h-[3.5vw];
		@apply text-[clamp(10px,1.07vw,1.07vw)] min-w-max;
	}

	.validator-mobile {
		@apply text-sm py-3;
		@apply flex justify-between;
		@apply border-b border-b-color-grey-border-line;
		@apply w-full;
	}

	.mobile-stuff {
		@apply w-full;
		@apply md:hidden;
	}

	.noborder {
		@apply border-transparent;
	}

	.item {
		@apply flex items-center gap-2;
	}

	.left {
		@apply flex flex-col gap-2;
	}

	.button {
		@apply w-full;
		@apply flex justify-center;
		@apply mt-[clamp(14px,1.6vw,1.6vw)];
	}
</style>

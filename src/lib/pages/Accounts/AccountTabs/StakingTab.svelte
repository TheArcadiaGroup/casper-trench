<script lang="ts">
	import Paginator from '$lib/components/Paginator/index.svelte';
	import EmptyIcon from '$lib/icons/EmptyIcon.svelte';
	import { millisToFormat, timeAgo } from '$utils/converters';
	import Switch from '$components/Reusables/Switch.svelte';
	import TransactionStatus from '$components/TableData/TransactionStatus.svelte';
	import { onMount } from 'svelte';
	import { getAccountDelegation, getAccountUndelegations } from '$utils/api';
	import { page } from '$app/stores';
	import { isLoading } from '$stores/loading';
	import type { Transaction } from '$utils/types/transaction';

	let transactions: Transaction[];
	let delegations: Transaction[];
	let undelegations: Transaction[];
	let itemsPerPage = 10;
	let startIndex = 1;
	let dataset: 'delegations' | 'undelegations' = 'delegations';
	const switchOptions = [
		{
			name: 'Staking History',
			dropdown: [],
			selectedDropdown: ''
		},
		{
			name: 'Unstaking History',
			dropdown: [],
			selectedDropdown: ''
		}
	];
	let selected = 0;

	$: if (selected === 0) {
		dataset = 'delegations';
		transactions = delegations;
	} else {
		dataset = 'undelegations';
		transactions = undelegations;
	}

	onMount(async () => {
		await fetchDelegations();
		await fetchUndelegations();
	});

	const fetchDelegations = async () => {
		$isLoading = true;
		delegations = await getAccountDelegation($page.params.address, itemsPerPage, startIndex);
		$isLoading = false;
	};

	const fetchUndelegations = async () => {
		$isLoading = true;
		undelegations = await getAccountUndelegations($page.params.address, itemsPerPage, startIndex);
		console.log(undelegations);
		$isLoading = false;
	};
	$: if (itemsPerPage) {
		setTimeout(async () => {
			dataset == 'delegations' ? await fetchDelegations() : await fetchUndelegations();
		}, 1);
	}
</script>

<div class="switch-container">
	<Switch outlined options={switchOptions} bind:selected />
</div>
<div class="staking-tab">
	<div class="total">
		Latest {itemsPerPage} Transactions
	</div>
	<table>
		<tr>
			<th class="blocky">Transaction</th>
			<th>Validator</th>
			<th class="right">TX Time</th>
			<th class="right">Amount</th>
			<th class="right">Status</th>
		</tr>
		<div class="divider table-header-border" />
		{#if transactions && transactions.length > 0}
			{#each transactions as transaction}
				<tr>
					<td class="blocky">
						<a class="hidden md:block" href="/transactions/{transaction.deployHash}">
							{transaction.deployHash}</a
						>
						<a class="md:hidden" href="/transactions/{transaction.deployHash}">
							{`${transaction.deployHash.substring(0, 5)}...${transaction.deployHash.substring(
								transaction.deployHash.length - 5
							)}`}</a
						></td
					>
					<td>
						<Validator hash={transaction.validator} />
					</td>
					<td class="time"
						>{timeAgo(millisToFormat(Date.now() - Date.parse(transaction.timestamp)))} ago</td
					>
					<td>
						<div class="value-crypto">
							<div class="crypto">
								{transaction.amount.toLocaleString()}
							</div>
							<div class="cspr">CSPR</div>
						</div>
					</td>
					<td>
						<div class="wrapper">
							<TransactionStatus success={transaction.status === 'success'}>
								{transaction.status}
							</TransactionStatus>
						</div>
					</td>
				</tr>
			{/each}
		{/if}
	</table>
	{#if transactions && transactions.length > 0}
		<Paginator
			showTotalRows={false}
			bind:itemsPerPage
			apiPaginator
			bind:items={transactions}
			bind:startIndex
			on:load-page={async () =>
				dataset == 'delegations' ? await fetchDelegations() : await fetchUndelegations()}
		/>
	{:else}
		<div class="empty">
			<div class="content">
				<div class="empty-icon">
					<EmptyIcon />
				</div>
				<div class="text">Empty</div>
			</div>
		</div>
	{/if}
</div>

<style lang="postcss">
	table {
		@apply table-auto w-full relative;
	}

	.divider {
		@apply h-[clamp(1px,0.18vw,0.18vw)] w-full;
		@apply absolute;
	}

	th {
		@apply py-[clamp(8px,0.5vw,0.5vw)] px-[clamp(16px,2vw,2vw)];
		@apply text-[clamp(14px,1.07vw,1.07vw)] font-normal text-color-table-header;
		@apply text-left;
	}

	td {
		@apply py-[clamp(8px,1.19vw,1.19vw)] px-[clamp(16px,2vw,2vw)];
		@apply text-[clamp(14px,1.07vw,1.07vw)] text-color-table-header min-w-max;
	}

	td.blocky {
		@apply text-color-hover-footer-link text-opacity-100 hover:text-opacity-70;
	}

	.blocky {
		@apply px-0;
	}

	.total {
		@apply text-[clamp(12px,1.07vw,1.07vw)] text-color-grey-footer-label;
		@apply mb-[2.38vw];
	}

	.right {
		@apply text-right;
	}

	.time {
		@apply align-top;
		@apply min-w-max;
	}

	.value-crypto {
		@apply flex items-center justify-end gap-[clamp(2px,0.24vw,0.24vw)];
		@apply text-right text-[clamp(10px,0.83vw,0.83vw)];
	}

	.crypto {
		@apply text-[clamp(14px,1.07vw,1.07vw)] text-color-table-header;
	}

	.cspr {
		@apply text-color-grey-footer-label;
	}

	.wrapper {
		@apply flex justify-end;
	}

	.switch-container {
		@apply flex justify-center;
		@apply mb-[clamp(4px,1.31vw,1.31vw)];
	}

	.empty-icon {
		@apply w-[clamp(20px,2.08vw,2.08vw)] h-[clamp(20px,2.08vw,2.08vw)];
	}

	.empty {
		@apply h-[23.81vh] md:h-[23.81vw] w-full;
		@apply flex items-center justify-center;
		@apply text-[clamp(14px,1.07vw,1.07vw)] text-color-grey-footer-label text-opacity-50;
	}

	.empty > div {
		@apply flex flex-col items-center justify-center;
	}
</style>

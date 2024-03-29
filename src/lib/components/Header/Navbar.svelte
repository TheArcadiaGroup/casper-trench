<script lang="ts">
	import CasperTrenchLogo from '$lib/icons/CasperTrenchLogo.svelte';
	import UserIcon from '$lib/icons/UserIcon.svelte';
	import NavbarDropdown from './NavbarDropdown.svelte';
	import { page } from '$app/stores';
	import { account } from '$stores/account';
	import Hash from '../TableData/Hash.svelte';
	import { disconnectWallet } from '$utils/wallets/connection';
	import MobileNavMenuIcon from '$lib/icons/MobileNavMenuIcon.svelte';
	import MobileNavMenu from './MobileNavMenu.svelte';
	import { slide } from 'svelte/transition';

	let navItems: {
		text: string;
		link: string;
		dropdown: {
			text: string;
			link: string;
		}[];
	}[] = [
		{
			text: 'Home',
			link: '/',
			dropdown: []
		},
		{
			text: 'Blockchain',
			link: '',
			dropdown: [
				{
					text: 'Top Accounts',
					link: '/accounts/top-accounts'
				},
				{
					text: 'View Txns',
					link: '/transactions'
				},
				{
					text: 'View Blocks',
					link: '/blocks'
				},
				{
					text: 'Contracts',
					link: '/contracts'
				}
			]
		},
		{
			text: 'Validators',
			link: '/validators',
			dropdown: []
		},
		{
			text: 'CSPR Live Charts & Stats',
			link: '/charts',
			dropdown: []
		}
		// {
		// 	text: 'More',
		// 	link: '',
		// 	dropdown: [
		// 		{
		// 			text: 'Developers',
		// 			link: ''
		// 		},
		// 		{
		// 			text: 'Wallet Analyzer',
		// 			link: ''
		// 		},
		// 		{
		// 			text: 'Explore',
		// 			link: ''
		// 		}
		// 	]
		// },
		// TODO uncomment if wallet functionality is needed -->
		// {
		// 	text: 'Wallet',
		// 	link: '',
		// 	dropdown: [
		// 		{
		// 			text: 'Transfer',
		// 			link: '/wallet/transfer'
		// 		},
		// 		{
		// 			text: 'Delegate Stake',
		// 			link: '/wallet/delegate'
		// 		},
		// 		{
		// 			text: 'Undelegate Stake',
		// 			link: '/wallet/undelegate'
		// 		}
		// 	]
		// }
	];

	let openNavBackdrop = false;
	let isHovering = false;
</script>

{#if openNavBackdrop}
	<MobileNavMenu handleClose={() => (openNavBackdrop = false)} {navItems} />
{/if}

<div class="navbar">
	<a href="/" class="logo" class:opacity-0={openNavBackdrop}>
		<CasperTrenchLogo />
	</a>
	<div
		class="nav-items"
		class:isHovering
		on:mouseenter={() => (isHovering = true)}
		on:mouseleave={() => (isHovering = false)}
	>
		{#each navItems as navItem}
			<div class="nav-item fillup">
				{#if navItem.dropdown.length > 0}
					<NavbarDropdown {navItem} {isHovering} />
				{:else}
					<div class="text" class:selected={$page.url.pathname === navItem.link}>
						<a href={navItem.link}>{navItem.text}</a>
					</div>
				{/if}
			</div>
		{/each}
		<!-- TODO uncomment if sign in functionality is needed -->
		<!-- {#if $account}
			<button on:click={disconnectWallet}>
				<Hash hash={$account.publicKey} />
			</button>
		{:else}
			<a href="/sign-in" class="signin">
				<div class="user-icon">
					<UserIcon />
				</div>
				<div class="text">Sign In</div>
			</a>
		{/if} -->
	</div>
	{#if !openNavBackdrop}
		<div class="nav-menu-mobile" on:click={() => (openNavBackdrop = true)} transition:slide>
			<div class="icon">
				<MobileNavMenuIcon />
			</div>
		</div>
	{/if}
</div>

<style lang="postcss">
	.navbar {
		@apply flex justify-between items-center mb-3 md:mb-0;
		@apply mx-4 mt-4 md:mx-0 md:mt-0;
	}

	.nav-items {
		@apply hidden md:flex md:items-center gap-[clamp(16px,1.79vw,1.79vw)];
		@apply text-white text-opacity-50 text-[clamp(10px,0.83vw,0.83vw)] transition-all;
	}

	.nav-item {
		@apply flex items-center cursor-pointer min-w-max;
		@apply transition-all duration-200;
	}

	.logo {
		@apply h-[40px] w-[110px] md:h-[clamp(16px,2.44vw,2.44vw)] md:w-[clamp(16px,7vw,7vw)];
		@apply transition-all;
	}

	/* .chevron {
		@apply h-[clamp(14px,0.95vw,0.95vw)] w-[clamp(14px,0.95vw,0.95vw)] ml-[clamp(4px,0.36vw,0.36vw)];
	} */

	.selected {
		@apply text-white text-opacity-100;
	}

	.signin {
		@apply flex items-center cursor-pointer;
		@apply text-white text-opacity-50 hover:text-opacity-70;
	}

	.user-icon {
		@apply h-[clamp(14px,1.07vw,1.07vw)] w-[clamp(14px,1.07vw,1.07vw)] mr-[clamp(4px,0.36vw,0.36vw)];
	}

	.icon {
		@apply w-6;
	}

	.nav-menu-mobile {
		@apply md:hidden;
	}

	.isHovering > div {
		@apply opacity-70;
	}

	.fillup {
		@apply hover:opacity-100;
	}
</style>

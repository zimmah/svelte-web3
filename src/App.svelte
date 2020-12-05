<script>
	let currentAccount;
	window.ethereum
		?.request({ method: "eth_accounts"} )
		.then(handleAccountsChanged)
		.catch(err => console.error(err));

	window.ethereum?.on("accountsChanged", handleAccountsChanged);
	window.ethereum?.on("chainChanged", () => window.location.reload());
		
	function handleAccountsChanged(accounts) {
		if (accounts.length === 0) {
			console.log("Please connect to MetaMask");
		} else if (accounts[0] !== currentAccount) {
			currentAccount = accounts[0];
		}
	}

	function connect() {
	ethereum
		.request({ method: "eth_requestAccounts" })
		.then(handleAccountsChanged)
		.catch(err => {
			if (err.code === 4001) {
				// EIP-1193 userRejectedRequest error
				// If this happens, the user rejected the connection request.
				console.log("Please connect to MetaMask.");
			} else {
				console.error(err);
			}
		});
	}
</script>

<main>
	{#if window.ethereum}
		<h1>Hello web3!</h1>
		<button on:click={connect}>Connect</button>
	{:else}
		<h1>Your browser is not ready for web3, please use Brave browser or install the metamask extension</h1>
	{/if}
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
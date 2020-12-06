<script>
    import {ethers} from 'ethers'
    import apikeys from '../../api-keys.json'

    const provider = ethers.getDefaultProvider(Number(window.ethereum?.chainId) || 0x1, apikeys);

    let currentAccount
    window.ethereum
    .on("accountsChanged", handleAccountsChanged)
    .on("chainChanged", () => window.location.reload())
    .request({ method: "eth_accounts"} )
    .then(handleAccountsChanged)
    .catch(err => console.error(err))
        
    function handleAccountsChanged(accounts) {
        if (accounts.length === 0) {
            console.log("Please connect to MetaMask")
        } else if (accounts[0] !== currentAccount) {
            currentAccount = accounts[0];
        }
    }

    function connect() {
        window.ethereum
        .request({ method: "eth_requestAccounts" })
        .then(handleAccountsChanged)
        .catch(err => {
            if (err.code === 4001) {
                // EIP-1193 userRejectedRequest error
                // If this happens, the user rejected the connection request.
                console.log("Please connect to MetaMask.")
            } else {
                console.error(err)
            }
        })
    }
</script>

<main>
    <h1>Hello web3!</h1>
    {#if currentAccount}
        <p>Connected to {currentAccount}</p>
    {:else}
        <button on:click={connect}>Connect</button>
    {/if}
</main>

<style>
        h1 {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 4em;
        font-weight: 100;
    }
</style>
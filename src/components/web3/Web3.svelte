<script lang="ts">
    import {ethers, Signer} from 'ethers'
import SendWeth from './interactions/sendWETH.svelte'
    import WrongNetwork from './WrongNetwork.svelte'

    const networks = {
        1: 'Main Ethereum Network',
        3: 'Ropsten Test Network',
        4: 'Rinkeby Test Network',
        5: 'Görli Test Network',
        42: 'Kovan Test Network',
    }

    const signer:Signer = (new ethers.providers.Web3Provider(window.ethereum)).getSigner()

    let currentAccount:string
    let connectedNetwork:string

    window.ethereum
    .on("accountsChanged", handleAccountsChanged)
    .on("chainChanged", () => window.location.reload())
    .on("disconnect", handleAccountsChanged)
    .request({ method: "eth_accounts"} )
    .then(handleAccountsChanged)
    .catch(err => console.error(err))
        
    function handleAccountsChanged(accounts:Array<string>):void {
        if (accounts.length === 0) {
            console.log("Please connect to MetaMask")
            currentAccount = undefined
        } else if (accounts[0] !== currentAccount) {
            currentAccount = accounts[0]
            connectedNetwork = networks[Number(window.ethereum.chainId)]
        }
    }

    function connect():void {
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
    {#if Number(window.ethereum.chainId) > 1}
        <h1>Hello web3!</h1>
        {#if currentAccount}
            <p>Connected to {currentAccount} at the {connectedNetwork}</p>
            <SendWeth {signer} />
        {:else}
            <button on:click={connect}>Connect</button>
        {/if}
    {:else}
        <WrongNetwork/>
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

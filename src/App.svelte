<script lang="ts">
    import Web3 from './components/web3/Web3.svelte'
    import NoWeb3 from './components/web3/NoWeb3.svelte'
</script>

<main>
    {#if window.ethereum}
        <Web3 />
    {:else}
        <NoWeb3 />
    {/if}
</main>

<style>
    main {
        text-align: center;
        padding: 1em;
        max-width: 240px;
        margin: 0 auto;
    }

    @media (min-width: 640px) {
        main {
            max-width: none;
        }
    }
</style>
<script lang="ts">
    import type { Signer } from 'ethers';
    import { IERC20 } from '../../../abi/abiExporter'
    import TransactionHandler from '../utils/TransactionHandler.svelte'

    const address:string = '0xc778417e063141139fce010982780140aa0cd5ab'
    export let receiver:string = '0x16Ec7612d92774fc144d3C3e751a0B6dD251335f'
    export let amount = BigInt(1000000000000000000)
    export let signer:Signer
</script>

<style></style>

<main>
    <label for="receiver">Reciever: <input bind:value={receiver} name="reciever" type="text"/></label>
    <label for="amount">Amount: <input bind:value={amount} name="amount" type="number"/></label>
    <TransactionHandler abi={IERC20} {signer} {address} method="transfer" args={[receiver, amount]}><p>Here you can have a custom component or snippet after a transaction is succesfull</p></TransactionHandler>
</main>
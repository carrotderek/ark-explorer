<template>
  <div>
    <div v-for="block in blocks" :key="block.id" class="tx-row-mobile px-5 py-4">
      <div class="border-b py-4 flex justify-between flex-wrap">
        <div>ID</div>
        <block-link :id="block.id"></block-link>
      </div>

      <div class="border-b py-4 flex justify-between flex-wrap">
        <div>Height</div>
        <div>{{ block.height }}</div>
      </div>

      <div class="border-b py-4 flex justify-between flex-wrap">
        <div>Timestamp</div>
        <div>{{ readableTimestamp(block.timestamp) }}</div>
      </div>

      <div class="border-b py-4 flex justify-between flex-wrap">
        <div>Transactions</div>
        <div>{{ block.numberOfTransactions }}</div>
      </div>

      <div class="border-b py-4 flex justify-between flex-wrap">
        <div>Generated by</div>
        <wallet-link :public-key="block.generatorPublicKey"></wallet-link>
      </div>

      <div class="border-b py-4 flex justify-between flex-wrap">
        <div>Amount (ARK)</div>
        <div>{{ readableCrypto(block.totalForged) }}</div>
      </div>

      <div class="py-4 flex justify-between flex-wrap">
        <div>Fee (ARK)</div>
        <div>{{ readableCrypto(block.totalFee) }}</div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import Currency from '@/components/utils/Currency'
import WalletLink from '@/components/links/Wallet'
import BlockLink from '@/components/links/Block'

export default {
  components: { Currency, BlockLink, WalletLink },

  props: {
    blocks: {
      type: Array,
      required: true,
    }
  }
}
</script>

<style scoped>
.tx-row-mobile:nth-child(even) {
  background-color: var(--color-theme-table-row);
}
</style>

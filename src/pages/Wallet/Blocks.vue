<template>
  <div class="max-w-2xl mx-auto pt-5">
    <content-header>Blocks</content-header>
    <section class="bg-theme-content-background shadow-theme xl:rounded-lg py-10">
      <div class="hidden sm:block">
        <table-blocks :blocks="blocks"></table-blocks>
      </div>
      <div class="sm:hidden">
        <table-blocks-mobile :blocks="blocks"></table-blocks-mobile>
      </div>
      <paginator :start="+this.page"></paginator>
    </section>
  </div>
</template>

<script type="text/ecmascript-6">
import ContentHeader from '@/components/ContentHeader'
import TableBlocks from '@/components/tables/Blocks'
import TableBlocksMobile from '@/components/tables/mobile/Blocks'
import Paginator from '@/components/utils/Paginator'
import WalletService from '@/services/wallet'
import BlockService from '@/services/block'

export default {
  components: {
    ContentHeader,
    TableBlocks,
    TableBlocksMobile,
    Paginator,
  },

  data: () => ({ blocks: [] }),

  created() {
    this.$on('paginatorChanged', page => this.changePage(page))
  },

  beforeRouteEnter (to, from, next) {
    WalletService
      .find(to.params.address)
      .then(wallet => BlockService.getByPublicKey(wallet.publicKey, to.params.page))
      .then(blocks => next(vm => vm.setBlocks(blocks)))
      .catch(() => next({ name: '404' }))
  },

  beforeRouteUpdate (to, from, next) {
    this.blocks = []

    WalletService
      .find(to.params.address)
      .then(wallet => BlockService.getByPublicKey(wallet.publicKey, to.params.page))
      .then(blocks => this.setBlocks(blocks))
      .catch(() => next({ name: '404' }))
  },

  computed: {
    address() {
      return this.$route.params.address
    },
    page() {
      return this.$route.params.page
    },
  },

  methods: {
    setBlocks (blocks) {
      if (!blocks) return

      this.blocks = blocks
    },

    changePage(page) {
      this.$router.push({
        name: 'wallet-blocks',
        params: { address: this.address, page }
      })
    }
  }
}
</script>

<template>
  <div class="mx-2 mb-auto flex flex-col items-center border px-2 py-8">
    <div class="w-auto items-center lg:mx-20">
      <Avator :accountAddress="address" width="220" height="220" />
    </div>
    <div>
      <p>{{ name }}</p>
    </div>
    <div>
      <a href="/settings">Settings</a>
    </div>
  </div>
</template>

<script>
import { getAccount } from '../../fixtures/dev-for-apps'
import Avator from '../Members/Avator.vue'
import { store } from '../../store'

export default {
  name: 'AuthorSidebar',
  data() {
    console.log(store.state.wallet.walletAddress)
    return {
      walletAddress: store.state.wallet.walletAddress,
    }
  },
  computed: {
    address: () => {
      console.log('au:', store.state.wallet.walletAddress)
      return store.state.wallet.walletAddress
    },
  },
  async created() {
    const res = await getAccount(this.accountAddress)
    this.name = res[0]?.name
  },
  components: {
    Avator,
  },
}
</script>

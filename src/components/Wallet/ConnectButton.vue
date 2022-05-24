<template>
  <div class="rounded border px-4 py-2">
    <div v-if="walletAddress">
      <a href="/me">{{ walletAddress }}</a>
    </div>
    <div v-else v-on:click="connect">Connect Wallet</div>
  </div>
</template>

<script>
import Web3Modal from 'web3modal'
import { providers } from 'ethers'
import detectEthereumProvider from '@metamask/detect-provider'
import { whenDefined } from '@devprotocol/util-ts'

import { store } from '../../store'

export default {
  name: 'ConnectButton',
  data() {
    const modalProvider = new Web3Modal({
      providerOptions: {
        injected: {
          package: detectEthereumProvider(),
        },
      },
      cacheProvider: true,
    })
    return {
      modalProvider,
      walletAddress: store.state.wallet.walletAddress,
    }
  },
  async mounted() {
    const web3ForInjected = await detectEthereumProvider()
    if (!web3ForInjected) {
      this.web3Modal.clearCachedProvider()
      return
    }

    if (
      this.modalProvider.cachedProvider === 'injected' &&
      web3ForInjected.selectedAddress
    ) {
      const connectedProvider = await this.modalProvider.connect()
      const newProvider = whenDefined(
        connectedProvider,
        (p) => new providers.Web3Provider(p)
      )

      const currentAddress = await newProvider.getSigner().getAddress()
      this.walletAddress = currentAddress
    }
  },
  methods: {
    async connect() {
      const connectedProvider = await this.modalProvider.connect()
      const newProvider = whenDefined(
        connectedProvider,
        (p) => new providers.Web3Provider(p)
      )
      store.commit('wallet/setProvider', { provider: newProvider })

      const currentAddress = await newProvider.getSigner().getAddress()
      this.walletAddress = currentAddress
      store.commit('wallet/setWalletAddress', { walletAddress: currentAddress })
    },
  },
}
</script>

<template>
  <header
    class="text-black dark:border-b-0 dark:text-white dark:shadow-xl burger:flex burger:items-center burger:justify-between burger:px-4 burger:py-3"
  >
    <div
      class="flex w-1/4 flex-wrap items-center justify-between px-4 py-3 align-baseline burger:flex-nowrap burger:p-0"
    >
      <a href="/" class="py-4 text-gray-900 dark:text-white md:p-4">D Club</a>
    </div>
    <ConnectButton client:only />
  </header>
</template>

<script>
import ConnectButton from '../Wallet/ConnectButton.vue'

export default {
  components: {
    ConnectButton,
  },
}
</script>

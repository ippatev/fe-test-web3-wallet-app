<script lang="ts">
import { defineComponent, ref, computed } from 'vue'
import { ethers } from 'ethers'

export default defineComponent({
  name: 'WalletConnect',
  setup() {
    const connected = ref(false)
    const address = ref('')
    const bnbBalance = ref(0)
    const testTokenBalance = ref(0)

    const shortAddress = computed(() => {
      if (!address.value) return ''
      return `${address.value.slice(0, 6)}...${address.value.slice(-4)}`
    })

    const connectWallet = async () => {
      try {
        // await window.ethereum.request({ method: 'eth_requestAccounts' })
        const provider = new ethers.providers.Web3Provider(window.ethereum)
        await provider.send('eth_requestAccounts', [])
        const signer = provider.getSigner()
        const address = await signer.getAddress()
        const bnbBalance = await provider.getBalance(address)
        const testTokenBalance = await provider.getBalance(address)

        connected.value = true
        address.value = address
        bnbBalance.value = ethers.utils.formatEther(bnbBalance)
        testTokenBalance.value = ethers.utils.formatEther(testTokenBalance)
      } catch (error) {
        console.error(error)
      }
    }

    return {
      connected,
      address,
      bnbBalance,
      testTokenBalance,
      connectWallet,
    }
  },
})
</script>

<template>
  <div class="flex justify-center items-center h-screen">
    <button
      v-if="!connected"
      @click="connectWallet"
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
    >
      Connect Wallet
    </button>
    <div v-else class="text-center">
      <p>Connected Wallet: {{ shortAddress }}</p>
      <p>BNB Balance: {{ bnbBalance }} BNB</p>
      <p>Test Token Balance: {{ testTokenBalance }} TEST</p>
    </div>
  </div>
</template>

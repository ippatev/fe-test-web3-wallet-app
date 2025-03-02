<script lang="ts">
import { defineComponent, ref, computed } from 'vue'
import Web3 from 'web3'

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
      if (window.ethereum) {
        try {
          await window.ethereum.request({ method: 'eth_requestAccounts' })
          const web3 = new Web3(window.ethereum)
          const accounts = await web3.eth.getAccounts()
          address.value = accounts[0]

          const balance = await web3.eth.getBalance(address.value)
          bnbBalance.value = web3.utils.fromWei(balance, 'ether')

          const testTokenContractAddress = '0xE7eaCC0aDD959e7619eAaAA069a2e4fcb4079715'
          const testTokenABI = [
            {
              inputs: [
                {
                  internalType: 'uint256',
                  name: 'initialSupply',
                  type: 'uint256',
                },
              ],
              stateMutability: 'nonpayable',
              type: 'constructor',
            },
            {
              inputs: [
                {
                  internalType: 'address',
                  name: 'spender',
                  type: 'address',
                },
                {
                  internalType: 'uint256',
                  name: 'allowance',
                  type: 'uint256',
                },
                {
                  internalType: 'uint256',
                  name: 'needed',
                  type: 'uint256',
                },
              ],
              name: 'ERC20InsufficientAllowance',
              type: 'error',
            },
            {
              inputs: [
                {
                  internalType: 'address',
                  name: 'sender',
                  type: 'address',
                },
                {
                  internalType: 'uint256',
                  name: 'balance',
                  type: 'uint256',
                },
                {
                  internalType: 'uint256',
                  name: 'needed',
                  type: 'uint256',
                },
              ],
              name: 'ERC20InsufficientBalance',
              type: 'error',
            },
            {
              inputs: [
                {
                  internalType: 'address',
                  name: 'approver',
                  type: 'address',
                },
              ],
              name: 'ERC20InvalidApprover',
              type: 'error',
            },
            {
              inputs: [
                {
                  internalType: 'address',
                  name: 'receiver',
                  type: 'address',
                },
              ],
              name: 'ERC20InvalidReceiver',
              type: 'error',
            },
            {
              inputs: [
                {
                  internalType: 'address',
                  name: 'sender',
                  type: 'address',
                },
              ],
              name: 'ERC20InvalidSender',
              type: 'error',
            },
            {
              inputs: [
                {
                  internalType: 'address',
                  name: 'spender',
                  type: 'address',
                },
              ],
              name: 'ERC20InvalidSpender',
              type: 'error',
            },
            {
              anonymous: false,
              inputs: [
                {
                  indexed: true,
                  internalType: 'address',
                  name: 'owner',
                  type: 'address',
                },
                {
                  indexed: true,
                  internalType: 'address',
                  name: 'spender',
                  type: 'address',
                },
                {
                  indexed: false,
                  internalType: 'uint256',
                  name: 'value',
                  type: 'uint256',
                },
              ],
              name: 'Approval',
              type: 'event',
            },
            {
              anonymous: false,
              inputs: [
                {
                  indexed: true,
                  internalType: 'address',
                  name: 'from',
                  type: 'address',
                },
                {
                  indexed: true,
                  internalType: 'address',
                  name: 'to',
                  type: 'address',
                },
                {
                  indexed: false,
                  internalType: 'uint256',
                  name: 'value',
                  type: 'uint256',
                },
              ],
              name: 'Transfer',
              type: 'event',
            },
            {
              inputs: [
                {
                  internalType: 'address',
                  name: 'owner',
                  type: 'address',
                },
                {
                  internalType: 'address',
                  name: 'spender',
                  type: 'address',
                },
              ],
              name: 'allowance',
              outputs: [
                {
                  internalType: 'uint256',
                  name: '',
                  type: 'uint256',
                },
              ],
              stateMutability: 'view',
              type: 'function',
            },
            {
              inputs: [
                {
                  internalType: 'address',
                  name: 'spender',
                  type: 'address',
                },
                {
                  internalType: 'uint256',
                  name: 'value',
                  type: 'uint256',
                },
              ],
              name: 'approve',
              outputs: [
                {
                  internalType: 'bool',
                  name: '',
                  type: 'bool',
                },
              ],
              stateMutability: 'nonpayable',
              type: 'function',
            },
            {
              inputs: [
                {
                  internalType: 'address',
                  name: 'account',
                  type: 'address',
                },
              ],
              name: 'balanceOf',
              outputs: [
                {
                  internalType: 'uint256',
                  name: '',
                  type: 'uint256',
                },
              ],
              stateMutability: 'view',
              type: 'function',
            },
            {
              inputs: [],
              name: 'decimals',
              outputs: [
                {
                  internalType: 'uint8',
                  name: '',
                  type: 'uint8',
                },
              ],
              stateMutability: 'view',
              type: 'function',
            },
            {
              inputs: [],
              name: 'name',
              outputs: [
                {
                  internalType: 'string',
                  name: '',
                  type: 'string',
                },
              ],
              stateMutability: 'view',
              type: 'function',
            },
            {
              inputs: [],
              name: 'symbol',
              outputs: [
                {
                  internalType: 'string',
                  name: '',
                  type: 'string',
                },
              ],
              stateMutability: 'view',
              type: 'function',
            },
            {
              inputs: [],
              name: 'totalSupply',
              outputs: [
                {
                  internalType: 'uint256',
                  name: '',
                  type: 'uint256',
                },
              ],
              stateMutability: 'view',
              type: 'function',
            },
            {
              inputs: [
                {
                  internalType: 'address',
                  name: 'to',
                  type: 'address',
                },
                {
                  internalType: 'uint256',
                  name: 'value',
                  type: 'uint256',
                },
              ],
              name: 'transfer',
              outputs: [
                {
                  internalType: 'bool',
                  name: '',
                  type: 'bool',
                },
              ],
              stateMutability: 'nonpayable',
              type: 'function',
            },
            {
              inputs: [
                {
                  internalType: 'address',
                  name: 'from',
                  type: 'address',
                },
                {
                  internalType: 'address',
                  name: 'to',
                  type: 'address',
                },
                {
                  internalType: 'uint256',
                  name: 'value',
                  type: 'uint256',
                },
              ],
              name: 'transferFrom',
              outputs: [
                {
                  internalType: 'bool',
                  name: '',
                  type: 'bool',
                },
              ],
              stateMutability: 'nonpayable',
              type: 'function',
            },
          ]

          const testTokenContract = new web3.eth.Contract(testTokenABI, testTokenContractAddress)
          const testTokenBalance = await testTokenContract.methods.balanceOf(address.value).call()
          testTokenBalance.value = ethers.utils.formatEther(testTokenBalance)

          connected.value = true
        } catch (error) {
          console.error('Error connecting to wallet:', error)
          if (error.code === 4001) {
            console.error('User denied account access')
          } else {
            console.error('Unknown error:', error)
          }
        }
      } else {
        alert('MetaMask is not installed!')
      }
    }

    return {
      connected,
      shortAddress,
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
      <router-link to="/transfer" class="mt-4 block text-blue-500 hover:underline">
        Transfer Test Tokens
      </router-link>
    </div>
  </div>
</template>

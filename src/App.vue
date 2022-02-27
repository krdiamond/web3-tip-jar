<template>
  <div id="app">
    <form v-if="showTipForm">
      <label>Send me Eth!</label>
      <input v-model="ethValue">
      <button @click.prevent="submit($event)">Submit</button>
    </form>
    <div v-else>Please install an Eth wallet (ex: https://metamask.io/)</div>
  </div>
</template>

<script>
import Web3 from 'web3';

export default {
  name: 'App',
  components: {},
  data(){
    return {
      web3: new Web3(Web3.givenProvider),
      showTipForm: window.ethereum ? true : false,
      ethValue: "0.01",
    }
  },
  mounted() {},
  computed: {
    
  },
  methods: {
    async send(amount) {
      // get a list of accounts from metamask
      const accounts = await window.ethereum.request({ method: "eth_requestAccounts" })

      // turn the amount into the base unit of currency, wei
      const wei = this.web3.utils.toWei(amount, "ether")
      
      // if at least one account
      if (accounts.length > 0) {
        // set up a transaction from logged in account
        // to a set amount, with the value in hex format
        window.ethereum.request({ 
          method: "eth_sendTransaction",
          params: [{
            from: accounts[0],
            to: "0x039C46B3bDd3cE8F7FdE0BaaBA1b3295E56ba85d",
            value: this.web3.utils.toHex(wei)
          }]
        })
      }
    },
    submit() {
      // if a wallet installed
      if (window.ethereum) {
        // get the input and pass to send function
        this.send(this.ethValue)
      }
    },
  }
}
</script>

<style>

</style>

<template>
  <div id="app">
    <div>
      The Ukraine Goverment is accepting donations in Ether <a href="https://www.bloomberg.com/news/articles/2022-02-26/ukraine-soliciting-crypto-donations-after-russian-invasion" target="_blank">(Bloomberg)</a>
    </div>
    <div>
      <a href="https://etherscan.io/address/0x165CD37b4C644C2921454429E7F9358d18A45e14" target="_blank">Click here to see a log of all previous donations</a>
    </div>
    <form v-if="showTipForm">   
      <input v-model="ethValue">
      <button @click.prevent="submit($event)">Submit Ether Donation</button>
      <label>***Submitting this form will pop open your crypto wallet and allow you to send your Ether</label>
    </form>

    <div v-else class="form-alt" >Please install a crypto wallet in your browser on desktop to see the donation form (ex: https://metamask.io/)</div>
    <div><img src="./assets/govt-tweet.png"></div>
    <div><img src="./assets/vitalik-confirmation.png"></div>
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
            to: "0x165CD37b4C644C2921454429E7F9358d18A45e14",
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

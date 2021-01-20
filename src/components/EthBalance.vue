<template>
  <div>
    <md-card>
      <md-card-header>
        <div class="md-title">Get ETH Balance</div>
        <div class="md-subhead">
          <md-field :class="messageClass">
            <label>Address</label>
            <md-input v-model="address" required></md-input>
            <span class="md-error">{{errors}}</span>
          </md-field>
          <md-button v-if="!inProgress" class="md-icon-button md-raised md-primary" v-on:click="getBalance">
            <md-icon>keyboard_arrow_right</md-icon>
          </md-button>

          <h3 v-if="balance">{{ balance }} Gwei</h3>
          <md-progress-bar md-mode="query" v-if="inProgress"></md-progress-bar>
        </div>
      </md-card-header>
      <md-card-actions v-if="balance">
        <md-button class="md-button" v-on:click="reset">reset</md-button>
      </md-card-actions>

    </md-card>

  </div>
</template>

<script>
import Web3 from 'web3'

export default {
  name: 'eth-balance',
  data: () => ({
    address: '',
    balance: '',
    errors: '',
    inProgress: false,
    hasMessages: false
  }),
  computed: {
    messageClass () {
      return {
        'md-invalid': this.hasMessages
      }
    }
  },
  methods: {
    getBalance: function () {
      // ToDo: Do all sorts of address validation in a real app
      if (this.address) {
        const web3 = new Web3('wss://mainnet.infura.io/ws/v3/0863f8c185314ba2a74e8c9f70635ed9')
        this.inProgress = true
        web3.eth.getBalance(this.address)
          .then(balance => {
            this.balance = web3.utils.fromWei(balance, 'Gwei')
            this.inProgress = false
          })
          .catch(error => {
            this.errors = error
          })
      } else {
        this.hasMessages = true
        this.errors = 'please enter a valid address'
      }
    },
    reset: function () {
      this.balance = ''
      this.address = ''
      this.error = ''
      this.validated = false
      this.inProgress = false
      this.hasMessages = false
    }
  }
}
</script>

<style scoped>
</style>

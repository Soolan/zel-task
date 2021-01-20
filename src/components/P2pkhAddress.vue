<template>
  <div>
    <md-card>
      <md-card-header>
        <div class="md-title">Get P2PKH Address</div>
        <div class="md-subhead">
          <md-field :class="messageClass">
            <label>Public Key</label>
            <md-input v-model="pubkey" required></md-input>
            <span class="md-error">{{ error }}</span>
          </md-field>
          <md-button class="md-icon-button md-raised md-primary" v-on:click="getP2PKH">
            <md-icon>keyboard_arrow_right</md-icon>
          </md-button>

          <h3 v-if="address">P2PKH Address: {{ address }}</h3>

        </div>
      </md-card-header>
      <md-card-actions v-if="address">
        <md-button class="md-button" v-on:click="reset">reset</md-button>
      </md-card-actions>
      <md-card-expand v-if="address">
        <md-card-actions md-alignment="space-between">
          <md-card-expand-trigger>
            <md-button class="md-icon-button">
              <md-icon>keyboard_arrow_down</md-icon>
            </md-button>
          </md-card-expand-trigger>
        </md-card-actions>

        <md-card-expand-content>
          <md-card-content>
            <div v-if="!inProgress && !validated">
              <h4>Validate The P2PKH Address (testnet)</h4>
              <md-button v-on:click="validate(address)" class="md-icon-button md-raised md-primary">
                <md-icon>spellcheck</md-icon>
              </md-button>
            </div>
            <div v-else>
              <h4>Address Found On Testnet
                <md-icon>check</md-icon>
              </h4>
            </div>
            <md-progress-bar md-mode="query" v-if="inProgress"></md-progress-bar>

          </md-card-content>
        </md-card-expand-content>
      </md-card-expand>
    </md-card>
  </div>
</template>

<script>
import * as bitcoin from 'bitcoinjs-lib'

export default {
  name: 'p2pkh-address',
  data: () => ({
    byteOffset: 'hex',
    pubkey: '',
    address: '',
    error: '',
    validated: false,
    inProgress: false,
    noError: null,
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
    getP2PKH: function () {
      // sample pub keys to test
      // 0250863ad64a87ae8a2fe83c1af1a8403cb53f53e486d8511dad8a04887e5b2352
      // 04ed04324c11a81a6ca52f519975044817bcf4333dab976d0907c3d9c0a7354a2be6254299b5d688ac78604a82e5b26602f92df5a8309bd61a3ed193ccde782fca
      const network = bitcoin.networks.testnet
      const pubkey = Buffer.from(this.pubkey, this.byteOffset)

      if (this.pubkey.length === 0) {
        // ToDo: perform other checks later. Needs a complete class to check valid hex, valid pub key, etc
        this.hasMessages = true
        this.error = 'Please enter a valid public key'
      } else {
        this.address = bitcoin.payments.p2pkh({pubkey, network}).address
      }
    },
    validate: function (address) {
      this.inProgress = true
      this.error = ''
      try {
        bitcoin.address.toOutputScript(address, bitcoin.networks.testnet)
        setTimeout(() => {
          this.inProgress = false
          this.validated = true
        }, 1300)
      } catch (e) {
        console.log(e)
        this.error = e.message
        this.validated = false
      }
    },
    reset: function () {
      this.pubkey = ''
      this.address = ''
      this.error = ''
      this.validated = false
      this.inProgress = false
      this.hasMessages = false
    }
  }
}
</script>

<style>
</style>

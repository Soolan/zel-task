<template>
  <div id="landing">
    <div class="md-layout-item md-layout md-gutter">
      <div class="md-layout-item md-size-33 md-small-size-100 md-xsmall-size-100">
        <block-height class="card"></block-height>
      </div>
      <div class="md-layout-item md-size-67 md-small-size-100 md-xsmall-size-100">
        <p2pkh-address class="card"></p2pkh-address>
        <eth-balance class="card"></eth-balance>
      </div>
    </div>
  </div>
</template>

<script>
import BlockHeight from './BlockHeight'
import P2pkhAddress from './P2pkhAddress'
import EthBalance from './EthBalance'
import Vue from 'vue'
import VueMaterial from 'vue-material'
import 'vue-material/dist/vue-material.min.css'
import 'vue-material/dist/theme/default.css'

Vue.use(VueMaterial)

export default {
  name: 'landing',
  components: {EthBalance, P2pkhAddress, BlockHeight}
}
</script>

<style>
.card {
  margin: 2rem;
}

h3, h4 {
  color: blue;
}

h1 {
  color: green;
  font-size: 2.4rem;
}
</style>

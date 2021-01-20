<template>
  <div>
    <md-card>
      <md-card-header>
        <div class="md-title">{{ data.name }} Block Height</div>
      </md-card-header>
      <md-card-content>
        <h1>{{ data.blocks }}</h1>
        <md-progress-spinner v-if="!data" md-mode="indeterminate"></md-progress-spinner>
      </md-card-content>
    </md-card>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'block-height',
  data: () => ({
    data: {},
    errors: []
  }),
  created () {
    axios.get('https://sochain.com/api/v2/get_info/BTC')
      .then(response => {
        this.data = response.data.data
      })
      .catch(e => this.errors.push(e))
  }
}
</script>

<style>
</style>

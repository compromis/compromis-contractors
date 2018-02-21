<template>
  <div>
    <h2>Anuncis oberts</h2>

    <open-bid v-for="bid in bids" :bid="bid" />
  </div>
</template>

<script>
import axios from 'axios'
import OpenBid from './OpenBid'

export default {
  name: 'open',
  components: {
    OpenBid
  },
  data () {
    return {
      bids: []
    }
  },
  mounted () {
    this.setOpenBids()
  },
  methods: {
    setOpenBids () {
      axios.get('https://compromis.net/espai/contractors/bids/open')
        .then((response) => {
          this.bids = response.data.data
          console.log(response)
        })
        .catch(function (error) {
          console.log(error)
        })
    }
  }
}
</script>

<style lang="scss">
</style>

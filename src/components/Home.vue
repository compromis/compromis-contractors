<template>
  <div>
    <open />
    <closed />
  </div>
</template>

<script>
import Open from './Open'
import Closed from './Closed'

export default {
  name: 'home',
  components: {
    Open,
    Closed
  },
  data () {
    return {

    }
  }
}
</script>

<style lang="scss">

</style>

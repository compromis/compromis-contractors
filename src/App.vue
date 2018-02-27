<template>
  <div id="app">
    <transition :name="transitionName" mode="out-in">
      <router-view class="child-view" />
    </transition>
  </div>
</template>

<script>
export default {
  name: 'App',

  data () {
    return {
      transitionName: 'slide-left'
    }
  },

  watch: {
    '$route' (to, from) {
      let transitionName = 'slide-left'
      if (to.path === '/') transitionName = 'slide-right'
      this.transitionName = transitionName
    }
  }
}
</script>

<style>
  #app {
    font-family: Compromis, sans-serif;
    line-height: 1.5;
  }

  .child-view {
    transition: all .5s cubic-bezier(.55,0,.1,1);
  }
  .slide-left-enter, .slide-right-leave-active {
    opacity: 0;
    -webkit-transform: translate(30px, 0);
    transform: translate(30px, 0);
  }
  .slide-left-leave-active, .slide-right-enter {
    opacity: 0;
    -webkit-transform: translate(-30px, 0);
    transform: translate(-30px, 0);
  }

  .slide-enter, .slide-leave-to {
      max-height: 0;
  }

  .slide-enter-to, .slide-leave {
      max-height: 500px;
  }

  .slide-enter-active,
  .slide-leave-active {
      overflow: hidden;
      transition: max-height .5s ease;
  }
</style>

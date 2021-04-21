<template>
  <div id="app">
    <MainNav v-if="deviceType === 1" />
    <transition name="fade"
                mode="out-in">
      <router-view/>
    </transition>
  </div>
</template>
<script>
import MainNav from '@/components/MainNav.vue'

export default {
  components: {
    MainNav
  },
  data () {
    return {
      deviceType: null // 1: PC 2: phone
    }
  },
  mounted () {
    this.deviceType = document.body.clientWidth > 500 ? 1 : 2
  }
}
</script>

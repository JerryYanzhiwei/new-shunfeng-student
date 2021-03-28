<template>
  <div id="app">
    <MainNav />
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
  }
}
</script>

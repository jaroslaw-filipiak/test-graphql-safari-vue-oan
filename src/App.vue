<template>
  <Header />
  <div class="empty-space"></div>
  <router-view />
  <CookieLaw />
</template>

<script>
import AOS from 'aos'
import Header from '@/components/Header.vue'
import CookieLaw from '@/components/CookieLaw.vue'

export default {
  components: {
    Header,
    CookieLaw
  },
  mounted() {
    AOS.init()
  }
}
</script>

<style lang="scss">
@import './assets/scss/oan.scss';

.empty-space {
  margin-top: 30px;
  height: 1px;
  padding-bottom: 30px;

  @include lg {
    margin-top: 65px;
    height: 1px;
    padding-bottom: 40px;
  }
}
</style>

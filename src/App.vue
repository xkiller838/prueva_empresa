<template>
  <div id="app">
    <Header></Header>
    <Partido></Partido>
    <Footer></Footer>
  </div>
</template>

<script>

import Header from './components/Header.vue'
import Partido from './components/Partido.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'App',
  components: {
    Header,
    Partido,
    Footer
  }
}
</script>



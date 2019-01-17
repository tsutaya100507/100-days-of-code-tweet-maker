<template>
  <div id="app">
    <Header />
    <Form />
    <TweetMessage />
    <Footer class="footer"/>
  </div>
</template>

<script>
import Header from './components/layouts/header.vue'
import Form from './components/layouts/form.vue'
import Footer from './components/layouts/footer.vue'

export default {
  name: 'app',
  components: {
    Header,
    Form,
    Footer,
  },
  data () {
    
  }
}
</script>

<style>
  @import './assets/reset.css';
  @import url('https://fonts.googleapis.com/css?family=M+PLUS+1p|ZCOOL+QingKe+HuangYou');
</style>

<style scoped>
  #app {
    position: relative;
    width: 100vw;
    min-height: 100vh;
    background-color: #000000;
    color: #00FF00;
  }

  .footer {
    position: absolute;
    bottom: 0;
  }
</style>

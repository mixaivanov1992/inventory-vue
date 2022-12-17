<template>
  <LeftMenu />
  <Container />
  <Footer />
</template>

<script>
import './assets/styles/reset.css';
import './assets/styles/style.css';
import LeftMenu from './components/LeftMenu/LeftMenu.vue';
import Container from './components/Container/Container.vue';
import Footer from './components/Footer/Footer.vue';

export default {
  name: 'App',
  components: {
    LeftMenu,
    Container,
    Footer
  }
}
</script>
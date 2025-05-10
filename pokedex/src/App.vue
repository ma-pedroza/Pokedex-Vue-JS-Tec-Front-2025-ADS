<script setup>
import Navbar from './components/NavbarComponent.vue'
import Footer from './components/FooterComponent.vue'
</script>

<template>
  <div>
    <Navbar></Navbar>
    <Footer></Footer>
    <router-view></router-view>
  </div>
</template>
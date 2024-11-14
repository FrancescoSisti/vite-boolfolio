<script setup>
import HelloWorld from './components/HelloWorld.vue'
import Header from './partials/Header.vue'
import Footer from './partials/Footer.vue'
import 'bootstrap/dist/css/bootstrap.min.css'
import 'bootstrap-icons/font/bootstrap-icons.css'
</script>

<template>
  <Header />
  <div class="container mt-4">
    <div class="row justify-content-center mb-4">
      <div class="col-auto">
        <a href="https://vite.dev" target="_blank">
          <img src="/vite.svg" class="logo" alt="Vite logo" />
        </a>
      </div>
      <div class="col-auto">
        <a href="https://vuejs.org/" target="_blank">
          <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
        </a>
      </div>
    </div>
    <HelloWorld msg="Vite + Vue" />
  </div>
  <Footer />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>

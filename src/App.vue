<script setup>
import { ref } from 'vue';

let num = ref(0);

</script>

<template>
  <!-- NAVBAR -->
  <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
    <div class="container-fluid">

      <router-link class="navbar-brand" to="/">
      <img src="https://cdn-icons-png.flaticon.com/512/188/188987.png" alt="Logo" width="30" height="24" class="d-inline-block align-text-top">
      PokeVite
      </router-link>
      
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup"
        aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav">
          <router-link class="nav-link" aria-current="page" to="/">Home</router-link>
          <router-link class="nav-link" to="/about">About</router-link>
        </div>
      </div>

    </div>
  </nav>

  <!-- CONTENT -->
  <div class="main">
    <router-view/>
  </div>

  <footer class="bg-primary text-light">
    <div>
     Design in {{ new Date().getFullYear() }}
    </div>
  </footer>
</template>

<style>
  body{
    height: 100vh;
  background: rgb(101,211,235);
  background: linear-gradient(0deg, rgba(101,211,235,0.9613095238095238) 0%, rgba(0,31,255,0.9725140056022409) 100%);
  }
  footer{
    position: fixed;
    bottom: 0;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 56px;
  }
</style>
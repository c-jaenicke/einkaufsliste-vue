<script setup>
import {RouterView} from 'vue-router'
</script>

<template>
  <header>
    <nav class="navbar navbar-dark navbar-expand-lg bg-body-tertiary">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">
          <img src="./assets/logo.svg" width="45">
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
                data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent"
                aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <a class="nav-link active" aria-current="page" href="/">Einkaufsliste</a>
            </li>
            <li class="nav-item">
              <a class="nav-link active" href="/about">Über</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </header>
  <RouterView/>
</template>

<style scoped>

</style>

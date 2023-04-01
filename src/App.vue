<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
    <header>
      <nav class="d-flex flex-row justify-content-around">
        <div class="vertical-align">
          <RouterLink to="/">
              <div class="h2 text-dark my-2">MAD</div>
            </RouterLink>
        </div>

        <div class="nav-buttons">
          <div class="vertical-align nav-buttons-link">
            <div class="_btn demo">DEMO</div>
          </div>
          <div class="vertical-align nav-buttons-link">
            <RouterLink to="/create">
              <div class="_btn">Création de fiche</div>
            </RouterLink>
          </div>
          <div class="vertical-align nav-buttons-link">
            <RouterLink to="/forms">
              <div class="_btn">Liste des fiches</div>
            </RouterLink>
          </div>
          <div class="vertical-align nav-buttons-link">
            <a href="https://www.linkedin.com/in/ulysse-sekpon-86342a176" target="_blank"><img class="nav-icon" src="@/assets/icons/linkedIn.png" alt="lien linkedin"></a>
          </div>
          <div class="vertical-align nav-buttons-link">
            <a href="https://github.com/ulysse-skpn" target="_blank"><img class="nav-icon" src="@/assets/icons/github.svg" alt="lien github"></a>
          </div>
        </div>
      </nav>
    </header>

  <RouterView />
</template>

<style scoped>

  header
  {
    background: rgb(255, 255, 255, 0.9);
    height: 2.5rem;
    position: fixed;
    width: 100%;
    z-index: 10;
  }

  nav
  {
    height: 100%;
  }
  .nav-buttons
  {
    display: flex;
  }

  .nav-buttons
  {
    display: flex;
    color: darkslategray;
    font-weight: bold;
  }
  .nav-buttons-link
  {
    border-bottom: 0.2rem grey solid;
    padding: 0.5rem;
    transition: 400ms ease-in-out;
  }
  .nav-buttons-link:hover
  {
    background-color: rgb(255, 0, 89);
    border-bottom: 0.2rem transparent solid;
  }
  .nav-buttons-link:hover img
  {
    filter: invert(100%) sepia(91%) saturate(38%) hue-rotate(321deg) brightness(110%) contrast(110%);
  }

  ._btn
  {
    cursor: pointer;
    color: darkslategray;
    transition: 400ms ease-in-out;
  }
  ._btn:hover
  {
    color: white;
  }

  .vertical-align
  {
    display: flex;
    align-items: center;
  }

  .nav-icon
  {
    height: 2rem;
  }

  .demo
  {
    font-size:2rem;
    color: darkslategray; /* Will override color (regardless of order) */
    transition: 500ms ease-in-out;
  }
  .demo:hover
  {
    -webkit-text-fill-color: white; /* Will override color (regardless of order) */
  }

</style>

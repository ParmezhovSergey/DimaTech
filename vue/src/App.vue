<script setup>
import { RouterLink, RouterView } from "vue-router";
</script>

<template>
  <header>
    <div class="wrapper">
      <nav>
        <RouterLink class="nav" to="/">Прогресс бар</RouterLink>
        <RouterLink class="nav" to="/chart">Круговая диаграмма</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
.wrapper {
  display: flex;
  height: 70px;
  background-color: rgb(38, 38, 251);
  align-items: center;
  font-size: 25px;
}
.nav {
  color: white;
  margin-left: 20px;
  text-decoration: none;
  border: 1px solid white;
  border-radius: 10px;
  padding: 5px;
}
.nav:hover {
  color: yellow;
  margin-left: 20px;
  text-decoration: none;
}
</style>

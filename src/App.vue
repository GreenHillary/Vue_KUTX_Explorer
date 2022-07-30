<template>
  <div id="app">
    <NavBar />
    <ListOfSongs />
  </div>
</template>

<script>

import NavBar from './components/NavBar'
import ListOfSongs from './components/ListOfSongs'

export default {
  name: 'App',
  components: {
    NavBar,
    ListOfSongs
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0px;
  font-size: 1.5rem;
  font-weight: 600;
}
</style>

<template>
  <div id="app">
    
    <router-view/>
  </div>
</template>

<style>
#app {
  @import url(https://fonts.googleapis.com/css?family=Roboto:300);
  @import url(https: //fonts.google.com/specimen/Playfair+Display?category=Serif);
  font-family: Roboto, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
}




nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #000000;
  background-color: #ffffff;
}
</style>

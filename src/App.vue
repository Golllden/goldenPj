<template>
  <div id="app">
    <nav>
      <router-link to="/">首頁</router-link> |
      <router-link to="/about">關於</router-link> |
      <router-link to="/calculate">試算</router-link>
    </nav>
    <router-view/>
  </div>
</template>

<style>



*{
    margin:0;
    padding:0;
    box-sizing: border-box;
}

body{
    padding: 15px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>

<template>
  <div id="app">
    <div class="navbar">
    <router-link to= '/'>
                 <img class="navbar--logo" src= "./assets/marvel2.png">
    </router-link>  

     <h1>LA VIDA ES MARVELILLOSA</h1>

     <div class="dedicatoria">
       <p class="dedicatoria--text">In memoriam:  </p>
       <img class="dedicatoria--imagen" src= "./assets/memorandum.jpg">
     </div>

    </div>

    <router-view/> 
  </div>
</template>

<style>
body{
  margin: 0px
}
html{
  min-height: 100vh;
  background-image: url("./assets/fondo-espacio5.jpg");
}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
.navbar{
  background-color: #bd1023;
  padding: 10px;
  display: flex;
  align-items: center;
  justify-content: space-between
}
h1{
  font-family: 'font', sans-serif;
  font-weight: bold;
  text-align: center;
  color: aliceblue;
  margin: 0px;
}
.navbar--logo{
 height: 37px;
  width: 100px;
}
.dedicatoria--imagen{
   height: 65px;
  width: 100px;
  border-radius: 10px;
}
.dedicatoria{
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}
.dedicatoria--text{
  margin-right: 10px;
  font-size: 16px;
  font-weight: bold;
  margin-top: 0px;
  margin-bottom: 0px;
  font-family: 'font', sans-serif;
  color: aliceblue;
}
</style>

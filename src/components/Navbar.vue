<template>
  <div id="nav">
    <router-link to="/" id="logo_url">
        <img :src="logo" :alt="alt" id="logo">
    </router-link>
    <router-link to="/">Inicio</router-link> 
    <router-link to="/pedidos">Pedidos</router-link>
  </div>
</template>

<script>
    export default {
        name: "Navbar",
        props: ["logo", "alt"]
    }
</script>

<style>
  #nav{
    border-bottom: 4px solid black;
    background-color: black;
    padding: 15px 50px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
  }

  #nav #logo_url{
    margin: auto;
    margin-left: 0;
  }


  #nav a {
    color: orange;
    text-decoration: none;
    margin: 12px;
    transform: .5s;
  }

  #nav a:hover{
    color: white;
  }
</style>
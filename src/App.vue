<template>
  <div id="nav">
    <div style="background: purple; color: white;padding: 30px" v-if="deferredPrompt" color="info" dark class="text-left">
      Get our free app. It won't take up space on your phone and also works
      offline!
      <template>
        <button  @click="dismiss">Dismiss</button>

        <button @click="install">Install</button>
      </template>
    </div>
    <div class="pa-4 text-center">
<!--      <img alt="Vue logo" src="./assets/logo.png" />-->
      <h1>Customize Your Vue.js PWA Installation</h1>
    </div>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </div>
  <router-view/>
</template>
<script>
import Cookies from 'js-cookie'
export default {
  name: 'App',
  data () {
    return {
      deferredPrompt: null
    }
  },
  created () {
    window.addEventListener('beforeinstallprompt', e => {
      e.preventDefault()
      // Stash the event so it can be triggered later.
      if (Cookies.get('add-to-home-screen') === undefined) {
        this.deferredPrompt = e
      }
    })
    window.addEventListener('appinstalled', () => {
      this.deferredPrompt = null
    })
  },
  methods: {
    async dismiss () {
      Cookies.set('add-to-home-screen', null, { expires: 15 })
      this.deferredPrompt = null
    },
    async install () {
      this.deferredPrompt.prompt()
    }
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
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

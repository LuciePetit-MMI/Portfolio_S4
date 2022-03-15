<template>
  <div id="app">
    <p class="absolute top-0 right-0 z-50">
      <router-link to="/" class="cursor-pointer">EN</router-link> / <router-link to="/Fr" class="cursor-pointer">FR</router-link>
    </p>
    <router-view/> 
  </div>
</template>

<script>

export default {
    name: 'App',
    data () {
        return {
        }
    }
}
</script>

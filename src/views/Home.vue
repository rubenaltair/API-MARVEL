<template>
  <div class="home">
    <comics/>
  </div>
</template>

<script>
// @ es una forma de abreviar /src, de aqui se llaman a todos los componentes que nos interesa en esste caso comic que es la pagina principal.
import comics from '@/components/comics.vue'

export default {
  name: 'home',
  components: {
    comics
  }
}
</script>

<style>
.home{
    padding-bottom: 40px;
}
</style>

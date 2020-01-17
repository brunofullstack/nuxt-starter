<template>
  <v-app dark>
    <v-layout column justify-center align-center>
      <v-flex xs12 sm8 md6>
        <div class="text-center">
          <h1 v-if="error.statusCode === 404">
            {{ pageNotFound }}
          </h1>
          <h1 v-else>
            {{ otherError }}
          </h1>
          <NuxtLink to="/">
            Voltar
          </NuxtLink>
        </div>
      </v-flex>
    </v-layout>
  </v-app>
</template>

<script>
  export default {
    layout: 'guest',
    props: {
      error: {
        type: Object,
        default: null
      }
    },
    head() {
      const title =
        this.error.statusCode === 404 ? this.pageNotFound : this.otherError
      return {
        title
      }
    },
    data() {
      return {
        pageNotFound: '404 Not Found',
        otherError: 'An error occurred'
      }
    }
  }
</script>

<style scoped>
  h1 {
    font-size: 20px;
  }
</style>

<template>
  <div>
    <div class="display-4">
      {{ title }}
    </div>

    <v-layout align-center>
      <v-flex text-xs-center>
        <v-btn flat href="https://laravel.com/docs">Documentation</v-btn>
        <v-btn flat href="https://laracasts.com">Laracasts</v-btn>
        <v-btn flat href="https://laravel-news.com">News</v-btn>
        <v-btn flat href="https://forge.laravel.com">Forge</v-btn>
        <v-btn flat href="https://github.com/laravel/laravel">GitHub</v-btn>
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  layout: 'basic',

  metaInfo () {
    return { title: this.$t('home') }
  },

  data: () => ({
    title: window.config.appName
  }),

  computed: mapGetters({
    authenticated: 'auth/check'
  })
}
</script>

<style scoped>
.top-right {
  position: absolute;
  right: 10px;
  top: 18px;
}

.title {
  font-size: 85px;
}
</style>

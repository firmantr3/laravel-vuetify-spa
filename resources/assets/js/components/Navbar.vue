<template>
  <div>
    <v-navigation-drawer
      fixed
      v-model="drawer"
      app
      v-if="user"
    >
      <v-list dense>
        <v-list-tile :ripple="true" :to="{ name: 'home' }" @click="">
          <v-list-tile-action>
            <v-icon>home</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>Home</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar app dark class="amber darken-3">
      <v-toolbar-side-icon @click="drawer = !drawer" v-if="user"></v-toolbar-side-icon>
      <v-toolbar-title>
        <router-link class="white--text brand" :to="{ name: user ? 'home' : 'welcome' }">{{ appName }}</router-link>
      </v-toolbar-title>
      <v-spacer></v-spacer>

      <v-toolbar-items class="hidden-sm-and-down">
        <locale-dropdown/>
        <template v-if="!user">
          <v-btn :to="{ name: 'login' }" flat>{{ $t('login') }}</v-btn>
          <v-btn :to="{ name: 'register' }" flat>{{ $t('register') }}</v-btn>
        </template>
        <template v-else>

          <v-menu offset-y>
            <v-btn slot="activator" flat>
              <v-avatar
                class="grey lighten-4"
              >
                <img :src="user.photo_url" alt="avatar">
              </v-avatar>
              <v-icon dark>arrow_drop_down</v-icon>
            </v-btn>
            <v-list>
              <v-list-tile :to="{ name: 'settings.profile' }">
                <v-list-tile-title v-text="$t('settings')"></v-list-tile-title>
              </v-list-tile>
              <v-list-tile @click.prevent="logout">
                <v-list-tile-title v-text="$t('logout')"></v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-menu>
        </template>

      </v-toolbar-items>
    </v-toolbar>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import LocaleDropdown from './LocaleDropdown'

export default {
  components: {
    LocaleDropdown
  },

  data: () => ({
    appName: window.config.appName,
    drawer: false
  }),

  computed: mapGetters({
    user: 'auth/user'
  }),

  methods: {
    async logout () {
      // Log out the user.
      await this.$store.dispatch('auth/logout')

      // Redirect to login.
      this.$router.push({ name: 'login' })
    }
  }
}
</script>

<style scoped>
.profile-photo {
  width: 2rem;
  height: 2rem;
  margin: -.375rem 0;
}
</style>

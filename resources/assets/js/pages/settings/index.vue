<template>
  <v-container grid-list-md>
    <v-layout row wrap>
      <v-flex md3>
        <card :title="$t('settings')" class="settings-card">
          <v-list>
            <v-list-tile v-for="tab in tabs" :key="tab.route" :to="{ name: tab.route }">
              <v-list-tile-action>
                <v-icon>{{ tab.icon }}</v-icon>
              </v-list-tile-action>
              <v-list-tile-content>
                {{ tab.name }}
              </v-list-tile-content>
            </v-list-tile>
          </v-list>
        </card>
      </v-flex>
      <v-flex md9>
        <transition name="fade" mode="out-in">
          <router-view/>
        </transition>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  middleware: 'auth',

  computed: {
    tabs () {
      return [
        {
          icon: 'account_circle',
          name: this.$t('profile'),
          route: 'settings.profile'
        },
        {
          icon: 'lock',
          name: this.$t('password'),
          route: 'settings.password'
        }
      ]
    }
  }
}
</script>

<style>
.settings-card .card-body {
  padding: 0;
}
</style>

<template>
  <v-app>
    <navbar/>

    <v-content>
      <v-container fluid>
        <child/>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import Navbar from '~/components/Navbar'

export default {
  name: 'MainLayout',

  components: {
    Navbar
  }
}
</script>

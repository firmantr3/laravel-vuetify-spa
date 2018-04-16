<template>
  <v-app>
    <navbar />

    <v-content>
      <v-container fluid fill-height>
        <v-layout justify-center align-center>
          <v-flex text-xs-center>
            <child/>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import Navbar from "~/components/Navbar";

export default {
  name: 'BasicLayout',

  components: {
    Navbar
  }
}
</script>

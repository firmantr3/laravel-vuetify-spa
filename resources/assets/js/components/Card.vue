<template>
  <v-card>
    <v-card-title class="pa-0" v-if="title">
      <v-subheader>{{ title }}</v-subheader>
    </v-card-title>
    <v-divider></v-divider>
    <v-card-text>
      <slot />
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  name: 'Card',

  props: {
    title: { type: String, default: null }
  }
}
</script>

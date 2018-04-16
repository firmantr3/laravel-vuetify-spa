<template>
  <v-scale-transition mode="out-in">
    <slot>
      <router-view/>
    </slot>
  </v-scale-transition>
</template>

<script>
export default {
  name: 'Child'
}
</script>

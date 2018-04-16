<template>
  <card :title="$t('your_password')">
    <v-form v-model="valid" @submit.prevent="update" @keydown="form.onKeydown($event)">
      <v-text-field
        :label="$t('password')"
        v-model="form.password"
        type="password"
        required
        :error="form.errors.has('password')"
        :error-messages="form.errors.get('password')"
      ></v-text-field>
      <v-text-field
        :label="$t('confirm_password')"
        v-model="form.password_confirmation"
        type="password"
        required
        :error="form.errors.has('password_confirmation')"
        :error-messages="form.errors.get('password_confirmation')"
      ></v-text-field>

      <v-btn type="submit" color="success" large :loading="form.busy">
        {{ $t('update') }}
      </v-btn>
    </v-form>
  </card>
</template>

<script>
import Form from 'vform'

export default {
  scrollToTop: false,

  metaInfo () {
    return { title: this.$t('settings') }
  },

  data: () => ({
    form: new Form({
      password: '',
      password_confirmation: ''
    }),
    valid: false
  }),

  methods: {
    async update () {
      await this.form.patch('/api/settings/password')

      this.form.reset()
    }
  }
}
</script>

<template>
  <card :title="$t('reset_password')">
    <v-form v-model="valid" @submit.prevent="reset" @keydown="form.onKeydown($event)">
      <alert-success :form="form" :message="status"/>

      <!-- Email -->
      <v-text-field
        :label="$t('email')"
        v-model="form.email"
        :rules="emailRules"
        required
        :error="form.errors.has('email')"
        :error-messages="form.errors.get('email')"
      ></v-text-field>

      <!-- Password -->
      <v-text-field
        :label="$t('password')"
        v-model="form.password"
        type="password"
        required
        :error="form.errors.has('password')"
        :error-messages="form.errors.get('password')"
      ></v-text-field>

      <!-- Password Confirmation -->
      <v-text-field
        :label="$t('confirm_password')"
        v-model="form.password_confirmation"
        type="password"
        required
        :error="form.errors.has('password_confirmation')"
        :error-messages="form.errors.get('password_confirmation')"
      ></v-text-field>

      <!-- Submit Button -->
      <v-btn type="submit" color="primary" large :loading="form.busy">
        {{ $t('reset_password') }}
      </v-btn>
    </v-form>
  </card>
</template>

<script>
import Form from 'vform'

export default {
  middleware: 'guest',

  metaInfo () {
    return { title: this.$t('reset_password') }
  },

  data: () => ({
    status: '',
    form: new Form({
      token: '',
      email: '',
      password: '',
      password_confirmation: ''
    }),
    valid: false,
    emailRules: [
      v => !!v || 'E-mail is required',
      v => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'
    ]
  }),

  created () {
    this.form.email = this.$route.query.email
    this.form.token = this.$route.params.token
  },

  methods: {
    async reset () {
      const { data } = await this.form.post('/api/password/reset')

      this.status = data.status

      this.form.reset()
    }
  }
}
</script>

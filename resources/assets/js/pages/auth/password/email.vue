<template>
  <card :title="$t('reset_password')">
    <v-form v-model="valid" @submit.prevent="send" @keydown="form.onKeydown($event)">
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

      <!-- Submit Button -->
      <v-btn type="submit" color="primary" large :loading="form.busy">
        {{ $t('send_password_reset_link') }}
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
      email: ''
    }),
    valid: false,
    emailRules: [
      v => !!v || 'E-mail is required',
      v => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'
    ]
  }),

  methods: {
    async send () {
      const { data } = await this.form.post('/api/password/email')

      this.status = data.status

      this.form.reset()
    }
  }
}
</script>

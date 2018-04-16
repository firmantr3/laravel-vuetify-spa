<template>
  <v-layout class="mt-4">
    <v-flex xs12 sm4 offset-sm4>
      <v-card>
        <v-card-title class="headline">
          {{ $t('register') }}
        </v-card-title>
        <v-card-text>
          <v-form v-model="valid" @submit.prevent="register" @keydown="form.onKeydown($event)">
            <v-text-field
              :label="$t('name')"
              v-model="form.name"
              required
              autofocus
              :error="form.errors.has('name')"
              :error-messages="form.errors.get('name')"
            ></v-text-field>
            <v-text-field
              :label="$t('email')"
              v-model="form.email"
              :rules="emailRules"
              required
              :error="form.errors.has('email')"
              :error-messages="form.errors.get('email')"
            ></v-text-field>
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

            <v-btn type="submit" color="primary" large :loading="form.busy">
              {{ $t('register') }}
            </v-btn>
            <!-- GitHub Register Button -->
            <login-with-github/>

          </v-form>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import Form from 'vform'
import LoginWithGithub from '~/components/LoginWithGithub'

export default {
  middleware: 'guest',

  components: {
    LoginWithGithub
  },

  metaInfo () {
    return { title: this.$t('register') }
  },

  data: () => ({
    form: new Form({
      name: '',
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

  methods: {
    async register () {
      // Register the user.
      const { data } = await this.form.post('/api/register')

      // Log in the user.
      const { data: { token } } = await this.form.post('/api/login')

      // Save the token.
      this.$store.dispatch('auth/saveToken', { token })

      // Update the user.
      await this.$store.dispatch('auth/updateUser', { user: data })

      // Redirect home.
      this.$router.push({ name: 'home' })
    }
  }
}
</script>

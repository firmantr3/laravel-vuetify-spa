<template>
  <v-layout class="mt-4">
    <v-flex xs12 sm4 offset-sm4>
      <v-card>
        <v-card-title class="headline">
          {{ $t('login') }}
        </v-card-title>
        <v-card-text>
          <v-form v-model="valid" @submit.prevent="login" @keydown="form.onKeydown($event)">
            <v-text-field
              :label="$t('email')"
              v-model="form.email"
              :rules="emailRules"
              required
              autofocus
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

            <v-layout row wrap justify-start align-start>
              <v-flex xs12 md6>
                <v-checkbox :label="$t('remember_me')" v-model="remember" name="remember"></v-checkbox>
              </v-flex>
              <v-spacer></v-spacer>
              <v-flex xs12 md6 text-xs-right>
                <router-link :to="{ name: 'password.request' }">
                  {{ $t('forgot_password') }}
                </router-link>
              </v-flex>
            </v-layout>

            <v-btn type="submit" color="primary" large :loading="form.busy">
              {{ $t('login') }}
            </v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import Form from 'vform'

export default {
  middleware: 'guest',

  metaInfo () {
    return { title: this.$t('login') }
  },

  data: () => ({
    form: new Form({
      email: '',
      password: '',
    }),
    remember: false,
    valid: false,
    emailRules: [
      v => !!v || 'E-mail is required',
      v => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'
    ]
  }),

  methods: {
    async login () {
      // Submit the form.
      const { data } = await this.form.post('/api/login')

      // Save the token.
      this.$store.dispatch('auth/saveToken', {
        token: data.token,
        remember: this.remember
      })

      // Fetch the user.
      await this.$store.dispatch('auth/fetchUser')

      // Redirect home.
      this.$router.push({ name: 'home' })
    }
  }
}
</script>

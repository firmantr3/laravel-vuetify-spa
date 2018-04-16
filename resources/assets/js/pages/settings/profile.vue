<template>
  <card :title="$t('your_info')">
    <v-form v-model="valid" @submit.prevent="update" @keydown="form.onKeydown($event)">
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

      <v-btn type="submit" color="success" large :loading="form.busy">
        {{ $t('update') }}
      </v-btn>
    </v-form>
  </card>
</template>

<script>
import Form from 'vform'
import { mapGetters } from 'vuex'

export default {
  scrollToTop: false,

  metaInfo () {
    return { title: this.$t('settings') }
  },

  data: () => ({
    form: new Form({
      name: '',
      email: ''
    }),
    valid: false,
    emailRules: [
      v => !!v || 'E-mail is required',
      v => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'
    ]
  }),

  computed: mapGetters({
    user: 'auth/user'
  }),

  created () {
    // Fill the form with user data.
    this.form.keys().forEach(key => {
      this.form[key] = this.user[key]
    })
  },

  methods: {
    async update () {
      const { data } = await this.form.patch('/api/settings/profile')

      this.$store.dispatch('auth/updateUser', { user: data })
    }
  }
}
</script>

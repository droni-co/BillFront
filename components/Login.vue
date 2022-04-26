<template>
  <div>
    <v-card :loading="loading">
      <template slot="progress">
        <v-progress-linear
          color="deep-purple"
          height="10"
          indeterminate
        ></v-progress-linear>
      </template>

      <v-card-title>Login</v-card-title>
      <v-alert
        dense
        outlined
        type="error"
        v-if="errors"
      >
        {{ errors.error }}
      </v-alert>
      <v-divider class="mx-4"></v-divider>
      <v-card-text>
        <v-text-field label="Email" type="email" v-model="loginUser.email" :rules="rules" hide-details="auto"></v-text-field>
        <v-text-field label="Password" type="password" v-model="loginUser.password" :rules="rules" hide-details="auto"></v-text-field>
      </v-card-text>
      
      <v-card-actions>
        <v-btn color="deep-purple lighten-2" @click="userLogin()" text>
          Login
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
  
</template>

<script>
export default {
  name: 'LoginComponent',
  data: () => ({
    loading: false,
    errors: null,
    rules: [
      value => !!value || 'Required.',
      value => (value && value.length >= 3) || 'Min 3 characters',
    ],
    loginUser: {
      email: '',
      password: '',
    },
  }),
  mounted() {
  },
  methods: {
    async userLogin() {
      this.loading = true;
      try {
        await this.$auth.loginWith('local', { data: this.loginUser })
        this.$router.push('/')
      } catch (err) {
        this.errors = err.response.data
        this.loading = false
      }
    }
  },
}
</script>

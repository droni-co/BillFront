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

      <v-card-title>Register</v-card-title>
      <v-alert
      dense
      outlined
      type="error"
      v-if="errors"
    >
      {{ errors.message }}
      <ul>
        <li v-for="(error, key) in errors.errors" :key="key">
          {{ key }}: {{ error }}
        </li>
      </ul>
    </v-alert>
      <v-divider class="mx-4"></v-divider>
      <v-card-text>
        <v-text-field label="Name" type="text" v-model="registerUser.name" :rules="rules" hide-details="auto"></v-text-field>
        <v-text-field label="Email" type="email" v-model="registerUser.email" :rules="rules" hide-details="auto"></v-text-field>
        <v-text-field label="Password" type="password" v-model="registerUser.password" :rules="rules" hide-details="auto"></v-text-field>
        <v-text-field label="Password confirmation" type="password" v-model="registerUser.password_confirmation" :rules="rules" hide-details="auto"></v-text-field>
      </v-card-text>
      
      <v-card-actions>
        <v-btn color="deep-purple lighten-2" @click="userRegister()" text>
          Register
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
  
</template>

<script>
export default {
  name: 'RegisterComponent',
  data: () => ({
    loading: false,
    errors: null,
    rules: [
      value => !!value || 'Required.',
      value => (value && value.length >= 3) || 'Min 3 characters',
    ],
    registerUser: {
      name: '',
      email: '',
      password: '',
      password_confirmation: '',
    },
  }),
  mounted() {
  },
  methods: {
    async userRegister() {
      this.loading = true;
      this.$axios.post('/api/register', this.registerUser)
        .then(response => {
          this.loading = false;
          this.userLogin();
        })
        .catch(error => {
          this.loading = false;
          this.errors = error.response.data;
        });
    },
    async userLogin() {
      this.loading = true;
      try {
        await this.$auth.loginWith('local', { data: this.registerUser })
        this.$router.push('/')
      } catch (err) {
        this.errors = err.response.data
        this.loading = false
      }
    }
  },
}
</script>

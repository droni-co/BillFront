<template>
  <v-btn color="deep-purple lighten-2" v-if="$nuxt.$route.path != '/login'" :loading="loading" @click="userLogout()" text>
    Logout
  </v-btn>  
</template>

<script>
export default {
  name: 'LogoutComponent',
  data: () => ({
    loading: false,
  }),
  mounted() {
  },
  methods: {
    async userLogout() {
      this.loading = true;
      try {
        await this.$auth.logout()
        this.$router.push('/login')
        this.loading = false;
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>

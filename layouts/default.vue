<template>
  <v-app dark>
    <v-app-bar fixed app>
      <v-btn icon>
        <v-icon>mdi-application</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <Logout />
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }} By Gustavo Barragan</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Bill System'
    }
  },
}
</script>

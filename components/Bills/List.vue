<template>
  <v-data-table
    :headers="headers"
    :items="bills"
    :items-per-page="10"
    class="elevation-1"
  >
    <template v-slot:[`item.number`]="{ item }">
      <v-chip blue :to="'/'+item.id">
        View #{{ item.number }}
      </v-chip>
    </template>
    <template v-slot:[`item.items`]="{ item }">
      <v-chip dark>
        {{ JSON.parse(item.items).length }}
      </v-chip>
    </template>
  </v-data-table>
</template>

<script>
export default {
  name: 'LogoutComponent',
  data: () => ({
    loading: false,
    headers: [
      {text: 'Number', value: 'number'},
      {text: 'Date', value: 'date'},
      {text: 'From', value: 'from_name'},
      {text: 'To', value: 'to_name'},
      {text: 'tems', value: 'items'},
      {text: 'subtotal', value: 'subtotal'},
      {text: 'Tax', value: 'tax'},
      {text: 'Total', value: 'total'},
    ],
    bills: [],
  }),
  mounted() {
    this.getBills()
  },
  methods: {
    getBills() {
      this.$axios.get('/api/bills')
        .then(response => {
          this.bills = response.data
        })
        .catch(error => {
          console.log(error)
        })
    }
  }
}
</script>

<template>
  <v-card :loading="loading">
    <v-card-title>Bill info</v-card-title>

    <v-card-text v-if="bill">
      <v-row>
        <v-col cols="12" sm="6">
          <v-text-field label="Number" type="number" v-model="bill.number" :rules="rules" hide-details="auto"></v-text-field>
          <v-text-field label="From name" type="text" v-model="bill.from_name" :rules="rules" hide-details="auto"></v-text-field>
          <v-text-field label="From document" type="text" v-model="bill.from_document" :rules="rules" hide-details="auto"></v-text-field>
          <v-text-field label="To name" type="text" v-model="bill.to_name" :rules="rules" hide-details="auto"></v-text-field>
          <v-text-field label="To document" type="text" v-model="bill.to_document" :rules="rules" hide-details="auto"></v-text-field>
          <v-date-picker class="mt-2" v-model="bill.date" :rules="rules"></v-date-picker>
        </v-col>
        <v-col cols="12" sm="6">
          <v-row class="mb-4">
            <v-col cols="8">Subtotal</v-col>
            <v-col cols="4" class="text-right">{{ bill.subtotal }}</v-col>
            <v-col cols="8">Tax</v-col>
            <v-col cols="4" class="text-right">{{ bill.subtotal * 0.19 }}</v-col>
            <v-col cols="8">Total</v-col>
            <v-col cols="4" class="text-right">{{ (bill.subtotal * 0.19) + bill.subtotal }}</v-col>
          </v-row>

          <h4>Add items</h4>
          <v-chip v-for="item in items" :key="item.name" class="me-3" @click="addItem(item)">
            {{ item.name }}
          </v-chip>

          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">Name</th>
                  <th class="text-left">Price</th>
                  <th class="text-left">Quantity</th>
                  <th class="text-left">Subtotal</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in bill.items" :key="item.name">
                  <td>{{ item.name }}</td>
                  <td>{{ item.price }}</td>
                  <td>{{ item.quantity }}</td>
                  <td>{{ item.quantity * item.price }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
          
        </v-col>
      </v-row>
    </v-card-text>
    <v-card-actions>
      <v-btn color="deep-purple lighten-2" @click="updateBill()" text>
        Update
      </v-btn>
      <v-btn color="deep-light lighten-2" to="/" text>
        Back
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  name: 'LogoutComponent',
  data: () => ({
    loading: false,
    rules: [
      v => !!v || 'Required',
    ],
    bill: null,
    items: [
      {
        name: 'Pc gamer',
        price: '10.00'
      },
      {
        name: 'Smartphone',
        price: '20.00',
      },
      {
        name: 'Mouse',
        price: '30.00',
      },
      {
        name: 'Keyboard',
        price: '40.00',
      },
      {
        name: 'Monitor',
        price: '50.00',
      },
    ],
  }),
  mounted() {
    this.getBill()
  },
  methods: {
    getBill() {
      this.$axios.get('/api/bills/'+this.$route.params.id)
        .then(response => {
          this.bill = response.data
          this.bill.items = JSON.parse(this.bill.items)
          this.bill.date = new Date(Date.parse(this.bill.date.substr(0, 10))).toISOString().substr(0, 10)
        })
        .catch(error => {
          console.log(error)
        })
    },
    addItem(item) {
      this.bill.items.find(i => i.name === item.name)
        ? this.bill.items.find(i => i.name === item.name).quantity++
        : this.bill.items.push({
          name: item.name,
          price: item.price,
          quantity: 1
        })
      this.calculateSubtotal()
    },
    calculateSubtotal() {
      this.bill.subtotal = this.bill.items.reduce((acc, item) => {
        return acc + (item.quantity * item.price)
      }, 0)
    },
    updateBill() {
      this.$axios.put('/api/bills/'+this.bill.id, this.bill)
        .then(response => {
          this.$router.push('/')
        })
        .catch(error => {
          this.errors = error.response.data
        })
    },
  }
}
</script>

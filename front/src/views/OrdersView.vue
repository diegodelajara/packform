<script>
import DatePicker from '@/components/DatePicker.vue'
import TotalAmount from '@/components/TotalAmount.vue'
import { orders, companies, customers, deliveries, items } from '../mocks'

export default {
  data() {
    return {
      search: '',
      headers: [
        { key: 'orderName', title: 'Orders name' },
        { key: 'customerCompany', title: 'Customer company' },
        { key: 'customerName', title: 'Customer Name' },
        { key: 'orderDate', title: 'Order date' },
        { key: 'deliveredAmount', title: 'Delivered Amount' },
      ],
      orders,
      companies,
      customers,
      deliveries,
      items,
      options: [{ value: 5, title: '5' }],
      total: [],
      date: '',
      fullOrders: [],
    }
  },
  computed: {
    getOrders() {
      const roundNumber = (price, quantity) => Math.round(price * quantity * 100) / 100
      const customersMap = new Map(customers.map((customer) => [customer.user_id, customer]))
      const companiesMap = new Map(companies.map((company) => [company.company_id, company]))

      const fullOrders = orders.map((order) => {
        const customer = customersMap.get(order.customer_id)
        const company = companiesMap.get(customer ? customer.company_id : null)
        const orderItems = items.find((item) => item.order_id === order.id)
        const total = roundNumber(orderItems.price_per_unit, orderItems.quantity)
        this.total.push(total)
        return {
          orderName: order.order_name,
          customerCompany: company.company_name,
          customerName: customer ? customer.name : null,
          orderDate: new Date(order.created_at),
          deliveredAmount: total ? `$ ${total}` : '-',
        }
      })
      return fullOrders
    },
  },
  components: {
    DatePicker,
    TotalAmount,
  },
  methods: {
    handleUpdateDate(date) {
      this.date = date
    },
    getTotalAmount() {
      return this.total.reduce((acc, value) => acc + value, 0)
    },
    getOrdersByDate() {
      if (this.date) {
        const [from, to] = this.date
        const start = new Date(from)
        const end = new Date(to)

        const ordersByDate = this.getOrders.filter(
          (order) => order.orderDate >= start && order.orderDate <= end,
        )

        this.fullOrders = ordersByDate
      }
    },
  },
  watch: {
    date() {
      this.getOrdersByDate()
    },
  },
  mounted() {
    this.fullOrders = !this.date ? this.getOrders : this.fullOrders
  },
}
</script>

<template>
  <v-card>
    <template v-slot:text>
      <v-container>
        <v-row cols="12" sm="12" no-gutters>
          <v-text-field
            v-model="search"
            label="Search"
            prepend-inner-icon="mdi-magnify"
            variant="outlined"
            single-line
          ></v-text-field>
        </v-row>
        <v-row>
          <v-col cols="12" sm="12" md="4">
            <date-picker @updateDate="handleUpdateDate"></date-picker>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12" sm="12" md="4">
            <total-amount :totalAmount="getTotalAmount()"></total-amount>
          </v-col>
        </v-row>
        <v-row>
          <v-data-table
            :headers="headers"
            :items="date ? fullOrders : getOrders"
            :search="search"
            :items-per-page-options="options"
            :items-per-page="5"
            first-icon="$first"
            last-icon="$last"
            prev-icon="$prev"
            next-icon="$next"
          ></v-data-table>
        </v-row>
      </v-container>
    </template>
  </v-card>
</template>

<style>
.v-card-text {
  min-height: 100vh;
}
</style>

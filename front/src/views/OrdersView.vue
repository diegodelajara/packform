<script>
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
      // data: [
      //   {
      //     name: 'Frozen Yogurt',
      //     calories: 159,
      //     fat: 6.0,
      //     carbs: 24,
      //     protein: 4.0,
      //     iron: 1,
      //   },
      //   {
      //     name: 'Ice cream sandwich',
      //     calories: 237,
      //     fat: 9.0,
      //     carbs: 37,
      //     protein: 4.3,
      //     iron: 1,
      //   },
      //   {
      //     name: 'Eclair',
      //     calories: 262,
      //     fat: 16.0,
      //     carbs: 23,
      //     protein: 6.0,
      //     iron: 7,
      //   },
      //   {
      //     name: 'Cupcake',
      //     calories: 305,
      //     fat: 3.7,
      //     carbs: 67,
      //     protein: 4.3,
      //     iron: 8,
      //   },
      //   {
      //     name: 'Jelly bean',
      //     calories: 375,
      //     fat: 0.0,
      //     carbs: 94,
      //     protein: 0.0,
      //     iron: 0,
      //   },
      // ],
      orders,
      companies,
      customers,
      deliveries,
      items,
      options: [
        { value: 2, title: '2' },
        { value: 5, title: '5' },
      ],
    }
  },
  computed: {
    getOrders() {
      const roundNumber = (price, quantity) => Math.round(price * quantity * 100) / 100
      const customersMap = new Map(customers.map((customer) => [customer.user_id, customer]))
      const companiesMap = new Map(companies.map((company) => [company.company_id, company]))

      return orders.map((order) => {
        const customer = customersMap.get(order.customer_id)
        const company = companiesMap.get(customer ? customer.company_id : null)
        const orderItems = items.find((item) => item.order_id === order.id)

        return {
          orderName: order.order_name,
          customerCompany: company.company_name,
          customerName: customer ? customer.name : null,
          orderDate: order.created_at,
          deliveredAmount: `$ ${roundNumber(orderItems.price_per_unit, orderItems.quantity)}`,
        }

        // const data = items.find((item) => item.order_id === order.id)
        // const newCustomers = customers.find((customer) => customer.login === order.customer_id)
        // const newCompanies = companies.find((company) =>
        //   customers.find((customer) => customer.company_id === company.company_id),
        // )
        // console.log('companies', newCompanies)

        // const newItems = items.find((item) => item.order_id === order.id)
        // const item = {
        //   orderName: order.order_name,
        //   customerCompany: newCompanies.company_name,
        //   customertName: newCustomers.name,
        //   orderDate: order.created_at,
        //   deliveredAmount: `$ ${newItems.price_per_unit}`,
        // }
        // return item
      })
    },
  },
}
</script>

<template>
  <v-card title="Orders" flat>
    <template v-slot:text>
      <v-text-field
        v-model="search"
        label="Search"
        prepend-inner-icon="mdi-magnify"
        variant="outlined"
        single-line
      ></v-text-field>
    </template>
    <v-data-table
      :headers="headers"
      :items="getOrders"
      :search="search"
      :items-per-page-options="options"
    ></v-data-table>
  </v-card>
  <v-icon icon="$vuetify"></v-icon>
</template>

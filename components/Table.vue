<template>
  <div class="mt-15">
    <v-data-table
      :headers="headers"
      :items="data"
      :options.sync="options"
      class="elevation-1"
    >
      <template v-slot:body>
        <tbody>
          <tr v-for="(item, i) in data" :key="i" @click="dialog(i)">
            <td>{{ item.name }}</td>
            <td>{{ item.username }}</td>
            <td>{{ item.email }}</td>
            <td>{{ item.street }}</td>
            <td>{{ item.city }}</td>
          </tr>
        </tbody>
        <v-dialog v-model="dialogShow" max-width="500px">
          <v-card>
            <v-card-title class="subheading font-weight-bold">
              information
            </v-card-title>

            <v-divider></v-divider>

            <v-list>
              <v-list-item v-for="(value, index) of dialogData" :key="index">
                <v-list-item-title>
                  <p>{{ index }} : {{ value }}</p>
                </v-list-item-title>
              </v-list-item>
            </v-list>
          </v-card>
        </v-dialog>
      </template>
    </v-data-table>
  </div>
</template>

<script>
export default {
  name: 'TablePage',

  data() {
    return {
      data: [],
      dialogShow: false,
      dialogData: {},
      options: {},
      headers: [
        {
          text: 'name',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'username', value: 'username' },
        { text: 'email', value: 'email' },
        { text: 'street', value: 'street' },
        { text: 'city', value: 'city' },
      ],
    }
  },
  watch: {
    options: {
      handler() {
        this.getData()
      },
      deep: true,
    },
  },
  methods: {
    dialog(i) {
      this.dialogShow = true
      this.dialogData = this.data[i]
    },
    async getData() {
      const axios = require('axios')

      const res = await axios.get('https://jsonplaceholder.typicode.com/users')

      const data = []
      let i = 0
      res.data.forEach((element) => {
        data[i] = {
          name: element.name,
          username: element.username,
          email: element.email,
          street: element.address.street,
          city: element.address.city,
          phone: element.phone,
          website: element.website,
          companyName: element.company.name,
        }
        i++
      })

      this.data = data
    },
  },
}
</script>

<template>
  <main class="ClientsList">
    <v-container>
      <v-form class="theForm">
        <v-text-field height="45px" v-model="searchValue" label="البحث"></v-text-field>
        <v-select height="45px" :items="selectItems" v-model="searchBy" label="بحث بواسطة" class="mr-10"></v-select>
        <v-icon large @click="$router.push({name: 'Home'})">fa fa-home</v-icon>
      </v-form>

      <v-row>
        <v-col lg="4" v-for="client in filteredClients" :key="client.cardNum">
          <v-card class="client" router :to="{name: 'Client', params: {client_id: client.cardNum}}">
            {{ client.clientName }}
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </main>
</template>

<script>
import db from '../firebase'

export default {
  data() {
    return {
      selectItems: [
        {text: 'الإسم', value: 'name'},
        {text: 'رقم التليفون', value: 'phone'},
        {text: 'رقم البطاقة', value: 'cardNum'}
      ],
      searchBy: 'name',
      searchValue: null,
      clients: [],
      filteredClients: []
    }
  },

  watch: {
    searchValue: {
      handler(val) {
        if(this.searchBy == 'name') {
          this.filteredClients = this.clients.filter(client => client.clientName.includes(val))
        }

        if(this.searchBy == 'phone') {
          this.filteredClients = this.clients.filter(client => client.phone.includes(val))
        }

        if(this.searchBy == 'cardNum') {
          this.filteredClients = this.clients.filter(client => client.cardNum.includes(val))
        }
      }
    }
  },

  mounted() {
    db.collection('clients').get()
      .then(res => {
        res.docs.forEach(doc => {
          this.clients.push(doc.data())
        })
        this.filteredClients = this.clients
        console.log(this.clients)
      })
  },
}
</script>

<style lang="scss">
.ClientsList {
  .theForm {
    display: grid;
    gap: 30px;
    grid-template-columns: 5fr 3fr .5fr;
  }

  .client {
    padding: 20px;
    display: grid;
    justify-content: center;
    align-items: center;
    font: 600 18px 'Cairo';
    color: #666;
    border: 3px solid #CCC;
    letter-spacing: 0 !important;
  }
}
</style>
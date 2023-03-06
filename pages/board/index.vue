<template>
  <v-container>
    <v-form @submit.prevent="sendMessage">
      <v-row>
        <v-col cols="12" sm="4">
          <v-text-field v-model="name" label="Name" />
        </v-col>
        <v-col cols="12" sm="8">
          <v-text-field v-model="text" label="Message" />
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-btn type="submit" color="primary">Send</v-btn>
        </v-col>
      </v-row>
    </v-form>
    <message-list :messages="displayedMessages" @message-deleted="deleteMessage" />
    <pagination :currentPage="currentPage" :totalPages="totalPages" @page-changed="changePage" />
  </v-container>
</template>

<script>
import firebase from 'firebase/compat/app';
import 'firebase/compat/database'
import MessageList from '@/components/MessageList'
import Pagination from '@/components/PaginationComponent'

const firebaseConfig = {
    apiKey: "AIzaSyCEw-ocIKup6SL9yAWMz7WrwcDQyb17uvE",
    authDomain: "fieldhawker-nuxt-test.firebaseapp.com",
    databaseURL: "https://fieldhawker-nuxt-test-default-rtdb.firebaseio.com",
    projectId: "fieldhawker-nuxt-test",
    storageBucket: "fieldhawker-nuxt-test.appspot.com",
    messagingSenderId: "102117116751",
    appId: "1:102117116751:web:6485f1c2a88ce09013933a",
    measurementId: "G-HS16SH5C2C"
  };

if (!firebase.apps.length) {
    firebase.initializeApp(firebaseConfig)
}

export default {
  components: {
    MessageList,
    Pagination
  },
  data() {
    return {
      name: '',
      text: '',
      messages: [],
      currentPage: 1,
      totalPages: 0
    }
  },
  async created() {
    await this.loadMessages()
  },
  computed: {
    displayedMessages() {
      const start = (this.currentPage - 1) * 10
      const end = start + 10
      return this.messages.slice(start, end)
    }
  },
  methods: {
    async loadMessages() {
      const messagesRef = firebase.database().ref('messages')
      const snapshot = await messagesRef.once('value')
      const messages = []
      snapshot.forEach(childSnapshot => {
        const message = childSnapshot.val()
        message.id = childSnapshot.key
        messages.push(message)
      })
      this.messages = messages.reverse()
      this.totalPages = Math.ceil(this.messages.length / 10)
    },
    async sendMessage() {
      const messagesRef = firebase.database().ref('messages')
      await messagesRef.push({
        name: this.name,
        text: this.text
      })
      this.name = ''
      this.text = ''
      await this.loadMessages()
    },
    async deleteMessage(id) {
      const messagesRef = firebase.database().ref('messages')
      await messagesRef.child(id).remove()
      this.messages = this.messages.filter(message => message.id !== id)
      this.totalPages = Math.ceil(this.messages.length / 10)
      if (this.currentPage > this.totalPages) {
        this.currentPage = this.totalPages
      }
    },
    changePage(page) {
      this.currentPage = page
    }
  }
}
</script>

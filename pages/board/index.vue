<template>
  <v-container>
    <v-form @submit.prevent="sendMessage">
      <v-row>
        <v-col cols="12" sm="4">
          <v-text-field v-model="name" label="Name*" />
        </v-col>
        <v-col cols="12" sm="8">
          <v-text-field v-model="text" label="Message*" />
        </v-col>
      </v-row>
      <v-btn type="submit" color="primary">Send</v-btn>
    </v-form>
    <v-card v-for="message in messages" :key="message.id" class="my-5">
      <v-card-title>{{ message.name }}</v-card-title>
      <v-card-text>{{ message.text }}</v-card-text>
      <v-card-actions>
        <v-btn color="error" @click="deleteMessage(message.id)">Delete</v-btn>
      </v-card-actions>
    </v-card>
  </v-container>
</template>

<script>
import firebase from 'firebase/compat/app';
import 'firebase/compat/database'

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
  data() {
    return {
      name: '',
      text: '',
      messages: []
    }
  },
  async created() {
    const messagesRef = firebase.database().ref('messages')
    await messagesRef.on('child_added', snapshot => {
      const message = snapshot.val()
      message.id = snapshot.key
      this.messages.push(message)
    })
  },
  methods: {
    sendMessage() {
      if (this.text !== '' && this.name !== '') {
        const messagesRef = firebase.database().ref('messages')
        messagesRef.push({
          name: this.name,
          text: this.text
        })
        this.name = ''
        this.text = ''
      }
    },
    deleteMessage(id) {
      const messagesRef = firebase.database().ref('messages')
      messagesRef.child(id).remove()
      this.messages = this.messages.filter(message => message.id !== id)
    }
  }
}
</script>

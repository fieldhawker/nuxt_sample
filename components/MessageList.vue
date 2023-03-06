<template>
  <v-card>
    <v-card-title>Messages</v-card-title>
    <v-card-text>
      <v-list>
        <v-list-item v-for="message in messages" :key="message.id">
          <v-list-item-content>
            <v-list-item-title>{{ message.name }}</v-list-item-title>
            <v-list-item-subtitle>{{ message.text }}</v-list-item-subtitle>
          </v-list-item-content>
          <v-list-item-action>
            <v-btn icon @click="deleteMessage(message.id)">
              <v-icon>mdi-delete</v-icon>
            </v-btn>
          </v-list-item-action>
        </v-list-item>
      </v-list>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: {
    messages: {
      type: Array,
      required: true
    }
  },
  methods: {
    deleteMessage(id) {
      this.$emit('message-deleted', id)
    }
  }
}
</script>

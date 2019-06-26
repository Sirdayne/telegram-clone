<template lang="pug">
#main-page 
  AppHeader
  #contacts
    .contact(v-for="contact in contacts" :key="contact.id" :class="{ active : selectedContactId === contact.id }" @click="selectContact(contact.id, contact)") 
      .avatar(:style="{ background: contact.color}")
      p {{ contact.name }}
  #chat
    #chat-container(v-if="selectedContactId")
      .message(v-for="(message, index) in selectedContact.messages" :key="index") 
        .avatar(:style="{ background: selectedContact.color}")
        p {{ message }} 

    .empty(v-else) Please select chat to start messaging
</template>

<script>

import AppHeader from '@/components/header'
import randomcolor from 'randomcolor'

export default {
  name: 'MainPage',
  data() {
    return {
      selectedContactId: null,
      selectedContact: {},
      contacts: [
        {
          id: 1,
          name: 'John Doe',
          messages: ["First John message", "Second John message", "Third John message"],
          color: ''
        },
        {
          id: 2,
          name: 'Sam Smith',
          messages: ["First Sam message", "Second Sam message", "Third Sam message"],
          color: ''
        },
        {
          id: 3,
          name: 'Lorem Ipsum',
          messages: ["First Lorem message", "Second Lorem message", "Third Lorem message"],
          color: ''
        },
      ]
    }
  },
  components: {
    AppHeader
  },
  computed: {
  
  },
  created() {
    this.getAvatarColors()
  },
  methods: {
    getAvatarColors() {
      this.contacts.forEach(contact => contact.color = this.getColor())
    },
    getColor() {
      return randomcolor({
        hue: 'orange'
      })
    },
    selectContact(id, contact) {
      this.selectedContactId = id
      this.selectedContact = contact
    }
  }
}
</script>

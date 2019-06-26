<template lang="pug">
#main-page 
  AppHeader
  #contacts
    .search
     input(v-model="search" placeholder="Search")  
    .contact(v-for="contact in filteredContacts" :key="contact.id" :class="{ active : selectedContact.id === contact.id }" @click="selectContact(contact.id, contact)") 
      .avatar(:style="{ background: contact.color}")
      p {{ contact.name }}
    .add-new-contact
     input(v-model="newContactName" v-on:keyup.enter="addNewContact()" placeholder="New contact")  
  #chat
    #chat-container(v-if="selectedContact.id")
      .message(v-for="(message, index) in selectedContact.messages" :key="index") 
        .avatar(:style="{ background: selectedContact.color}")
        .text
          .nickname {{ selectedContact.name }}
          p {{ message }} 
    .empty(v-else) Please select chat to start messaging
    .new-message(v-if="selectedContact.id")
      .avatar(:style="{ background: selectedContact.color}")
      input(v-model="newMessage" v-on:keyup.enter="addNewMessage()" placeholder="Write new message...")      
</template>

<script>

import AppHeader from '@/components/header'
import randomcolor from 'randomcolor'

export default {
  name: 'MainPage',
  data() {
    return {
      selectedContact: {id: null},
      search: '',
      newContactName: '',
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
      ],
      newMessage: ''
    }
  },
  components: {
    AppHeader
  },
  computed: {
    filteredContacts() {
      let array = this.contacts
      if (this.search && this.search.length > 0) {
        array = this.contacts.filter(contact => contact.name.toLowerCase().includes(this.search.toLowerCase()))
      }
      return array
    }
  },
  created() {
    this.getLocalStorage()
    this.getAvatarColors()
  },
  methods: {
    selectContact(id, contact) {
      this.selectedContact = contact
    },
    addNewMessage() {
      this.selectedContact.messages.push(this.newMessage)
      this.newMessage = ''
      this.setLocalStorage()
    },
    addNewContact() {
      const newContact = {
        id: this.contacts.length+1,
        name: this.newContactName,
        messages: [],
        color: this.getColor()
      }
      this.contacts.push(newContact)
      this.newContactName = ''
      this.setLocalStorage()
    },
    getLocalStorage() {
      this.contacts = localStorage.getItem('contacts') ? JSON.parse(localStorage.getItem('contacts')) : this.contacts;
    },
    setLocalStorage() {
      localStorage.setItem('contacts', JSON.stringify(this.contacts));
    },
    getAvatarColors() {
      this.contacts.forEach(contact => contact.color = this.getColor())
    },
    getColor() {
      return randomcolor({
        hue: 'orange'
      })
    }
  }
}
</script>

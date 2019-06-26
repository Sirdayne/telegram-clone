<template lang="pug">
#main-page 
  AppHeader
  #contacts
  #chat
</template>

<script>

import AppHeader from '@/components/header';

export default {
  name: 'MainPage',
  components: {
    AppHeader
  },
  computed: {
    getLogged() {
      return false
    },
  },
  created() {
    
  }
}
</script>

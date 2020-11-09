<template>
  <v-app style="light">
    <nav-bar></nav-bar>
    <v-row>
      <v-col>
        <new-user-form></new-user-form>
      </v-col>
      <v-col>
        <p>Total users: {{ users.length }}</p>
        <ul>
          <li v-for="user in users" :key="user.username">
            <v-avatar color="red">
              <span class="white--text headline">{{
                user.username.charAt(0)
              }}</span>
            </v-avatar>
            {{ user.username
            }}<span>({{ user.isonline ? 'ready' : 'busy' }})</span>
          </li>
        </ul>
      </v-col>
    </v-row>
  </v-app>
</template>

<script>
import Vue from 'vue'
import NavBar from '~/components/NavBar'
import NewUserForm from '~/components/NewUserForm'

import firebase from 'firebase/app'
// Required for side-effects
require('firebase/firestore')

// Initialize Cloud Firestore through Firebase

if (!firebase.apps.length) {
  firebase.initializeApp({
    apiKey: 'AIzaSyBLUgVecVPkHWfsR7U5I65Z7BG8_9lFp3c',
    authDomain: 'rufree-d8ad6.firebaseapp.com',
    projectId: 'rufree-d8ad6',
  })
}

var db = firebase.firestore()

export default Vue.extend({
  components: { NavBar },
  data() {
    return {
      users: [],
      timer: '',
    }
  },
  created() {
    this.fetchUsers()
    this.timer = setInterval(this.fetchUsers, 20000)
  },
  methods: {
    fetchUsers() {
      this.users = []
      db.collection('users')
        .get()
        .then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
            // console.log(`${doc.id} => ${doc.data()}`)
            // console.dir(doc.data())
            this.users.push(doc.data())
          })
        })
    },
    cancelAutoUpdate() {
      clearInterval(this.timer)
    },
  },
  beforeDestroy() {
    cancelAutoUpdate()
  },
})
</script>

<style scoped>
.a {
  font-size: 100px;
}
</style>

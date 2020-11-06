<template>
  <v-container>
    <v-form>
      <v-text-field
        v-model="username"
        :placeholder="`Enter your name`"
      ></v-text-field>
      <v-checkbox
        v-model="isonline"
        :label="`Online: ${isonline}`"
      ></v-checkbox>
      <v-btn @click="addStatus" color="teal">Update status</v-btn>
    </v-form>
  </v-container>
</template>

<script lang="ts">
import Vue from 'vue'
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
  data() {
    return {
      username: '',
      isonline: false,
    }
  },
  methods: {
    addStatus() {
      db.collection('users')
        .doc(this.username)
        .set({
          username: this.username,
          isonline: this.isonline,
        })
        .then(function () {
          console.log('Document successfully written!')
        })
        .catch(function (error) {
          console.error('Error writing document: ', error)
        })
    },
  },
})
</script>

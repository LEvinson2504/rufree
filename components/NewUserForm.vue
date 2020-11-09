<template>
  <v-container>
    <v-form>
      <v-checkbox
        v-model="isonline"
        :label="`Online: ${isonline}`"
      ></v-checkbox>
      <v-btn @click="addStatus" color="teal">Update status</v-btn>
      <v-btn @click="gLogin" color="teal">Login</v-btn>
      <h3>{{ username }}</h3>
    </v-form>
  </v-container>
</template>

<script lang="ts">
import Vue from 'vue'
import firebase from '../utilities/firebase'

var db = firebase.firestore()
// authentication
var provider = new firebase.auth.GoogleAuthProvider()

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
    gLogin() {
      firebase
        .auth()
        .signInWithPopup(provider)
        .then((result) => {
          // This gives you a Google Access Token. You can use it to access the Google API.
          var token = result.credential.accessToken
          // The signed-in user info.
          this.username = result.user?.displayName
          console.log(result.user?.displayName)
          // ...
        })
        .catch(function (error) {
          // Handle Errors here.
          var errorCode = error.code
          var errorMessage = error.message
          // The email of the user's account used.
          var email = error.email
          // The firebase.auth.AuthCredential type that was used.
          var credential = error.credential
          // ...
        })
    },
  },
})
</script>

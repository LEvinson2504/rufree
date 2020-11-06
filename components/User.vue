<template>
  <h1></h1>
</template>

<script lang="ts">
import Vue from 'vue'
var faunadb = require('faunadb'),
  q = faunadb.query

var client = new faunadb.Client({ secret: process.env.FAUNA_DB_SECRET })

export default Vue.extend({
  data() {
    return {
      users: [],
    }
  },
  asyncData(context) {
    client
      .query(q.Get(q.Ref(q.Collection('Users'))))
      .then((ret) => console.log(ret))
  },
})
</script>

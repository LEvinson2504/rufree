<template>
  <h1>Welcome {{ name }}</h1>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  data() {
    return {
      name: 'Levinson',
    }
  },
})
</script>

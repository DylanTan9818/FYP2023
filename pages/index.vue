<template lang="pug">
</template>

<script>
export default {
  name: 'IndexPage',
  layout: 'default',
  data () {
    return {
    }
  },
  created () {
    setTimeout(() => this.$router.push({ path: '/home' }), 1000)
  },
  methods: {

  }
}
</script>

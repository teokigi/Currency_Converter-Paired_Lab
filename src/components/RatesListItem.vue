<template lang="html">
  <div>
    {{rate}}, {{index}}
  </div>
</template>

<script>
export default {
  name: 'rates-list-item',
  props: ['rate'],
  methods: {
    displayRate: function() {
      return Object.keys(this.rate)[0]
    }
  }
}
</script>

<style lang="css" scoped>
</style>

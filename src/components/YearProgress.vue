<template>
  <div class="progressbar">
    <progress :percent="percent" activeColor="#EA5A49" show-info />
    <p>{{year}}年已经过去了{{days}}天</p>
  </div>
</template>

<script>
  export default {
    methods: {
      isLeapYear () {
        const year = new Date().getFullYear()
        if (year % 400 === 0) {
          return true
        } else if (year % 100 !== 0 && year % 4 === 0) {
          return true
        } else {
          return false
        }
      },
      getDayOfYear () {
        return this.isLeapYear() ? 366 : 365
      }
    },
    computed: {
      year () {
        return new Date().getFullYear()
      },
      days () {
        let start = new Date()
        start.setMonth(0)
        start.setDate(1)
        // start就是今年的第一天
        let offet = new Date().getTime() - start.getTime()
        return parseInt(offet / 1000 / 60 / 60 / 24 + 1)
      },
      percent () {
        return (this.days * 100 / this.getDayOfYear()).toFixed(1)
      }
    }
  }
</script>

<style lang="stylus" scoped>
  .progressbar
    width: 100%
    margin-bottom: 15px
</style>


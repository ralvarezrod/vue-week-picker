<template>
  <div class="container-fluid wpContainer">
    <div class="container-fluid wpHeaderContainer">
      <WPHeader :unixDate="date" :propWeek="week" @update:date="date = $event"></WPHeader>
    </div>
    <div class="container-fluid wpBodyContainer">
      <WPBody :unixDate="date" :propWeek="week" @update:week="week = $event"></WPBody>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
import WPHeader from './WPHeader'
import WPBody from './WPBody'
export default {
  data(){
    return {
      date: this.unixDate,
      week: this.propWeek
    }
  },
  props: {
    unixDate: {
      default: moment().unix(),
      type: Number
    },
    propWeek: {
      default: moment().isoWeek()
    }
  },
  components: {
    WPHeader,
    WPBody
  },
  watch: {
    unixDate: function(newVal, oldVal) { // watch it
      this.date = newVal;
    },
    propWeek: function(newVal, oldVal) { // watch it
      this.week = newVal;
    },
    date: function(newVal, oldVal) { // watch it
      this.$emit('update:date', newVal)
    },
    week: function(newVal, oldVal) { // watch it
      this.$emit('update:week', newVal)
    }
  },
  methods: {
  }
}
</script>

<style lang="scss">
  .wpContainer {
    display: grid;
    grid-template-rows: 100px 300px;
    width: 300px;
    border: solid 1px black;
    border-radius: 15px;
    overflow: hidden;
  }
</style>


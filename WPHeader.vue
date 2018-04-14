<template>
  <div class="wpHeader jail">
    <div class="backSelectorArea jail" v-on:click="changeMonth(-1)">
      <button class="button is-primary backSelector" ><i class="fa fa-chevron-left"></i></button>
    </div>
    <div class="headerArea jail">
      <div class="yearArea jail"><button class="coloredArea button is-primary">{{year}}</button></div>
      <div class="monthArea jail"><button class="coloredArea button is-primary">{{month}}</button></div>
    </div>
    <div class="nextSelectorArea jail">
      <button class="button is-primary backSelector" v-on:click="changeMonth(1)"><i class="fa fa-chevron-right"></i></button>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
export default {
  data(){
    return {
      localDate: this.unixDate
    }
  },
  props:{
    unixDate: {
      default: moment().unix(),
      type: Number
    }
  },
  computed: {
    month() {
      const date = moment.unix(this.localDate);
      let month = date.locale('es').format('MMMM');
      return month.charAt(0).toUpperCase() + month.slice(1);
    },
    year() {
      const date = moment.unix(this.localDate);
      return date.year();
    }
  },
  watch: {
    watch: { 
      unixDate: function(newVal, oldVal) { // watch it
        console.log('Prop changed: ', newVal, ' | was: ', oldVal)
      }
    }
  },
  methods: {
    changeMonth(number){      
      let date = moment.unix(this.localDate);
      date.add(number, 'months');
      this.localDate = date.unix();
      this.$emit('update:date', this.localDate);
    }
  }
}
</script>


<style lang="scss">
  .jail {
    overflow: hidden;
  }

  .wpHeader {    
    display: grid;
    grid-template-columns: 50px 200px 50px;

    .backSelector, .nextSelector{
      width: 100%;
      height: 100%;
      border-radius: 0px;
      font-size: 35px;
    }

    .headerArea {
      display: grid;
      grid-template-rows: 30px 70px;
      grid-template-columns: 200px;
      vertical-align: middle;
      justify-content: center;
      text-align: center;

      .coloredArea {
        width: 100%;
        height: 100%;
        border-radius: 0px;
        pointer-events: none;
      }

      .yearArea {
        .coloredArea {
          font-size: 20px;
          padding: 0px;
          margin: 0px;
          height: 100%;
          font-weight: bolder;
        }
      }

      .monthArea {
        
        .coloredArea {
          font-size:35px;
          height: 100%;
          font-weight: bolder;
          padding: 0px;
          padding-bottom: 10px;
        }
      }
    }
  }  
</style>


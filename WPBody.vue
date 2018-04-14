<template>
  <div class="wpBody">
    <div class="dayHeaders calendarRow" id="headerRow">
      <div class="headerCell"><button class="button headerItem">L</button></div>
      <div class="headerCell"><button class="button headerItem">M</button></div>
      <div class="headerCell"><button class="button headerItem">M</button></div>
      <div class="headerCell"><button class="button headerItem">J</button></div>
      <div class="headerCell"><button class="button headerItem">V</button></div>
      <div class="headerCell"><button class="button headerItem">S</button></div>
      <div class="headerCell"><button class="button headerItem">D</button></div>
    </div>
    <div class="calendarRow" v-for="(week, wkIdx) in calendar" :key="wkIdx" v-on:mouseover ="hoverIndex = wkIdx" v-on:mouseout="hoverIndex = null" v-on:click="setWeek(week.week)" :title="wkIdx">
      <div class="calendarCell" v-for="(cell, clIdx) in week.cells" :key="clIdx">
        <button :class="{button: true, calendarItem: true, inactive: !cell.active, 'is-info': wkIdx == hoverIndex && wkIdx != selectedIndex,'is-primary': wkIdx === selectedIndex}">{{cell.date.date()}}</button>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
export default {
  data(){
    return {
      date: this.unixDate,
      week: this.propWeek,
      hoverIndex: null,
    }
  },
  computed:{
    calendar(){
      const date = this.date;
      return this.createCalendar(date);
    },
    selectedIndex(){
      let index = this.week - this.dateMoment(this.date).clone().startOf('month').isoWeek();
      return index;
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
  watch: {
    unixDate: function(newVal, oldVal) { // watch it
      this.date = newVal;
    },
    propWeek: function(newVal, oldVal) { // watch it
      this.week = newVal;
    }    
  },
  methods:{
    
    dateMoment(date){
      return moment.unix(date)
    },

    setWeek(week){
      this.week = week;
      this.$emit('update:week', this.week);
    },

    createCalendar(date){
      const currentDate = moment.unix(date);
      const startWeek = currentDate.clone().startOf('month').isoWeek();
      const endWeek = currentDate.clone().endOf('month').isoWeek();
      const calendar = []

      for(var week = startWeek; week<= endWeek; week++){
        const storeWeek = week;
        const storeCells = [];
        const startDate = moment().isoWeek(week).startOf('isoWeek');
        for (let i = 0; i < 7; i++) {
          const auxDate = startDate.clone();
          const cell = {date: auxDate};
          auxDate.add(i, 'days');
          if(week === startWeek && auxDate.date()>20) {
            cell.active = false;
          }else if(week > startWeek + 1 && auxDate.date()<7){
            cell.active = false;
          }else{
            cell.active = true;
          }
          storeCells.push(cell);  
        }
        calendar.push({
          week:storeWeek,
          cells:storeCells
        })
      }
      return calendar;
    }
  }
}
</script>

<style lang="scss">
  .wpBody {
    height: 100%;
    display: grid;
    grid-template-rows: repeat(7, 1fr);

    .inactive {
      color: lightgray;
    }

    .calendarRowSelected {
      background-color: red;
      .calendarCell {
        background-color: red;
      }
    }    

    #headerRow {
      pointer-events: none;
    }

    .calendarRow{
      display: grid;
      grid-template-columns: repeat(7, 42.8px);
      text-align: center;
      cursor: pointer;

      .calendarCell {
        border-radius: 0px;
        pointer-events: none;

        .calendarItem {
          border-radius: 0px;
          pointer-events: none;
          width: 100%;
          height: 100%;
        }
      }

      .headerCell {
        width: 100%;
        height: 100%;

        .headerItem {
          width: 100%;
          height: 100%;
          border-radius: 0px;
          font-weight: bolder;
          pointer-events: none;
        }
      }
    }
  }
</style>


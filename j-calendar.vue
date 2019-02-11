<template>
  <div class="j-calendar-wrap">
    <div class="tool-box">
      <div>
        <button style="font-size: 16px;" @click="onClickToday">今天</button>
      </div>
      <div style="flex: 3;">
        <select @change="setYear" class="select-picker" name="j-calendar-year" id="j-calendar-year">
          <option v-for="i in 29" :selected="(fullYear - 29 + i) === fullYear" :value="fullYear - 29 + i" v-bind:key="i + 'lf'">{{fullYear - 29 + i}}</option>
          <option v-for="i in 30" :value="fullYear + i" v-bind:key="i + 'rf'">{{fullYear + i}}</option>
        </select>
        年
        <select @change="setMonth" class="select-picker" name="j-calendar-month" id="j-calendar-month">
          <option v-for="i in month" :selected="i === month" :value="i" v-bind:key="i + 'jf'">{{i}}</option>
          <option v-for="i in (12 - month)" v-bind:key="i + 'ef'" :value="i + month">{{i + month}}</option>
        </select>
        月
      </div>
      <div>
      </div>
    </div>
    <div class="week-box">
      <div>
        <span>
          周日
        </span>
      </div>
      <div>
        <span>
          周一
        </span>
      </div>
      <div>
        <span>
          周二
        </span>
      </div>
      <div>
        <span>
          周三
        </span>
      </div>
      <div>
        <span>
          周四
        </span>
      </div>
      <div>
        <span>
          周五
        </span>
      </div>
      <div>
        <span>
          周六
        </span>
      </div>
    </div>
    <div class="day-box">
      <div v-for="i in firstDay" class="day-wrap" v-bind:key="i + 'if'">
        <div>
        </div>
      </div>
      <div v-for="i in getDateAmount" @click.stop="setDate(i)" class="day-wrap" v-bind:key="i + 'hs'" :class="[ (i === date) && 'selected-day' ,isSameDay(i) && 'active-today']">
       <div>
         <div class="day-inner">
           {{i}}
         </div>
       </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      today: new Date(),
      selectedDate: new Date()
    }
  },
  computed: {
    day () {
      return this.selectedDate.getDay()
    },
    date () {
      return this.selectedDate.getDate()
    },
    month () {
      return this.selectedDate.getMonth() + 1
    },
    fullYear () {
      return this.selectedDate.getFullYear()
    },
    firstDay () {
      let { fullYear, month } = this
      return (new Date(`${fullYear}-${month}-01`)).getDay()
    },
    getDateAmount () {
      let { fullYear, month, date } = this
      let d = new Date(`${fullYear}-${month + 1}-${date}`)
      d.setDate(0)
      return d.getDate()
    }
  },
  methods: {
    setDate (d) {
      let self = this
      let { fullYear, month } = self
      self.$nextTick(() => {
        self.selectedDate = new Date(`${fullYear}-${month}-${d}`)
      })
    },
    setMonth (e) {
      let m = e.srcElement.value
      let self = this
      let { fullYear, date } = self
      self.$nextTick(() => {
        self.selectedDate = new Date(`${fullYear}-${m}-${date}`)
      })
    },
    setYear (e) {
      let y = e.srcElement.value
      let self = this
      let { month, date } = self
      self.$nextTick(() => {
        self.selectedDate = new Date(`${y}-${month}-${date}`)
      })
    },
    isSameDay (d) {
      let { today, month, fullYear } = this
      let d1 = today
      let d2 = new Date(`${fullYear}-${month}-${d}`)
      return (d1.getFullYear() === d2.getFullYear()) && (d1.getMonth() === d2.getMonth()) && (d1.getDate() === d2.getDate())
    },
    onClickToday () {
      this.selectedDate = new Date()
    }
  }
}
</script>

<style scope>
  * {
    padding: 0;
    margin: 0;
  }
  html {
    font-size: 62.5%;
  }
  body {
    font-size: 1.5em;
    line-height: 1.6;
    font-weight: 400;
    font-family: "Raleway", "HelveticaNeue", "Helvetica Neue", Helvetica, Arial, sans-serif;
    color: #222;
  }
  .j-calendar-wrap {
    width: 100%;
  }
  .j-calendar-wrap .tool-box {
    display: flex;
    padding: 15px 0;
    border-bottom: 1px solid #efefef;
    font-size: 18px;
  }
  .j-calendar-wrap .tool-box > div {
    flex: 1;
    text-align: center;
  }
  .j-calendar-wrap .week-box{
    display: flex;
    padding: 8px 0;
    border-bottom: 1px solid #efefef;
    font-size: 14px;
  }
  .j-calendar-wrap .week-box > div{
    flex: 1;
    text-align: center;
  }
  .j-calendar-wrap .day-box{
    display:flex;
    flex-wrap: wrap;
  }
  .j-calendar-wrap .day-wrap{
    width: calc(100% / 7);
    text-align: center;
  }
  .j-calendar-wrap .day-wrap > div{
    width: 100%;
    padding-bottom: 100%;
    position: relative;
  }
  .j-calendar-wrap .day-inner{
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
  .j-calendar-wrap .select-picker{
    text-align: center;
    display: inline;
    text-align-last:center;
    line-height: 28px;
    font-size: 18px;
  }
  .j-calendar-wrap .active-today{
    background-color: pink;
    color: #fff;
  }
  .j-calendar-wrap .selected-day{
    background-color: lightskyblue;
    color: #fff;
  }
</style>

<template>
  <div id="app">
    <form action @submit.prevent>
      <input type="text" v-model.lazy="searchDateStr" @keyup.enter="searchDate" />
      <select v-model="lang">
        <option :value="true">En</option>
        <option :value="false">Ru</option>
      </select>
    </form>
    <table class="table">
      <thead>
        <tr>
          <td>
            <button @click="decrease">-</button>
          </td>
          <td v-if="lang" colspan="5">{{monthes[0][month]}} {{year}}</td>
          <td v-else colspan="5">{{monthes[1][month]}} {{year}}</td>
          <td>
            <button @click="increase">+</button>
          </td>
        </tr>
        <tr v-if="lang">
          <td v-for="day in dayWeek[0]" :key="day">{{day}}</td>
        </tr>
        <tr v-else>
          <td v-for="day in dayWeek[1]" :key="day">{{day}}</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="week of calendar()" :key="week.index">
          <td v-for="(day, index) in week" :key="index" @click="choseDate(day.index)">{{day.index}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      lang: true,
      year: new Date().getFullYear(),
      month: new Date().getMonth(),
      dFirstMonth: '1',
      dayWeek: [
        ['Mn', 'Tu', 'We', 'Th', 'Fr', 'Sa', 'Su'],
        ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс']
      ],
      monthes: [
        ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"],
        ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь']
      ],
      searchDateStr: `${new Date().getFullYear()}.${new Date().getMonth()}.${new Date().getDate()}`,
    }
  },
  methods: {
    calendar: function () {
      let days = [];
      let week = 0;
      days[week] = [];
      let dlast = new Date(this.year, this.month + 1, 0).getDate();
      for (let i = 1; i <= dlast; i++) {
        if (new Date(this.year, this.month, i).getDay() != this.dFirstMonth) {
          let a = { index: i };
          days[week].push(a);
        } else {
          week++;
          days[week] = [];
          let a = { index: i };
          days[week].push(a);
        }
      }
      if (days[0].length > 0) {
        for (let i = days[0].length; i < 7; i++) {
          days[0].unshift('');
        }
      }
      return days;

    },
    decrease: function () {
      this.month--;
      if (this.month < 0) {
        this.month = 12;
        this.month--;
        this.year--;
      }
    },
    increase: function () {
      this.month++;
      if (this.month > 11) {
        this.month = -1;
        this.month++;
        this.year++;
      }
    },
    searchDate: function () {
      this.month = new Date(this.searchDateStr).getMonth()
      this.year = new Date(this.searchDateStr).getFullYear()
    },
    choseDate: function (day) {
      this.searchDateStr = `${this.year}.${this.month}.${day}`
    }
  }
}
</script>

<style scoped>
#app {
  margin-top: 100px;
  width: 400px;
  margin-left: auto;
  margin-right: auto;
}

.table {
  border-collapse: collapse;
  float: left;
  width: 280px;
  table-layout: fixed;
}

.format-week {
  float: right;
}

.table td {
  text-align: center;
}

.table td:hover {
  cursor: pointer;
  background: #dedada;
}

.table thead tr:last-child {
  background-color: #dedada;
}
</style>
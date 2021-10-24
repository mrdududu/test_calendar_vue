<template>
  <div class="cal">
    <div class="cal-header">
      <div class="cal-header-nav" @click="setPrevMonth">
        <i class="arrow left"></i>
      </div>
      <div class="cal-header-title">{{ monthName }} {{ year }}</div>
      <div class="cal-header-nav" @click="setNextMonth">
        <i class="arrow right"></i>
      </div>
    </div>
    <table class="cal-table">
      <thead>
        <tr>
          <th v-for="(date, jdx) in calendar[0]" :key="`wnames_${jdx}`">
            {{ getWeekdayName(date) }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(week, idx) in calendar" :key="`week_${idx}`">
          <td
            v-for="(date, jdx) in week"
            :key="`date_${jdx}`"
            :class="{
              withmonth: 1 === date.getDate(),
              othermonth: month !== date.getMonth(),
            }"
          >
            <div v-if="1 === date.getDate()" class="td-month">
              {{ getMonthName(date) }}
            </div>
            <div>
              {{ date.getDate() }}
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
export default {
  props: {
    initialYear: {
      type: Number,
      default() {
        return new Date().getFullYear();
      },
    },
    initialMonth: {
      type: Number,
      default() {
        return new Date().getMonth();
      },
    },
  },
  data() {
    return {
      year: this.initialYear,
      month: this.initialMonth,
    };
  },
  computed: {
    monthName() {
      return new Date(this.year, this.month, 1).toLocaleString("default", {
        month: "long",
      });
    },
    daysInMonth() {
      return new Date(this.year, this.month + 1, 0).getDate();
    },
    firstDay() {
      return new Date(this.year, this.month, 1);
    },
    lastDay() {
      return new Date(this.year, this.month, this.daysInMonth);
    },
    firstWeekday() {
      return this.firstDay.getDay();
    },
    lastWeekday() {
      return this.lastDay.getDay();
    },
    calendar() {
      const calGrid = [];
      let week = [];

      for (
        let i = 0 - this.firstWeekday;
        i < 6 - this.lastWeekday + this.daysInMonth;
        i++
      ) {
        const dayDate = new Date(
          this.firstDay.getTime() + 24 * 60 * 60 * 1000 * i
        );
        week.push(dayDate);

        if (6 === dayDate.getDay()) {
          calGrid.push(week);
          week = [];
        }
      }
      return calGrid;
    },
  },
  methods: {
    getWeekdayName(date) {
      return date.toLocaleDateString("default", { weekday: "short" });
    },
    getMonthName(date) {
      return date.toLocaleString("default", { month: "short" });
    },
    setNextMonth() {
      if (11 === this.month) {
        this.month = 0;
        this.year++;
        return;
      }

      this.month++;
    },
    setPrevMonth() {
      if (0 === this.month) {
        this.month = 11;
        this.year--;
        return;
      }

      this.month--;
    },
  },
};
</script>
<style lang="css" scoped>
@import url("https://fonts.googleapis.com/css2?family=Roboto&display=swap");
.arrow {
  border: solid #666;
  border-width: 0 2px 2px 0;
  display: inline-block;
  padding: 4px;
}
.right {
  transform: rotate(-45deg);
  -webkit-transform: rotate(-45deg);
}
.left {
  transform: rotate(135deg);
  -webkit-transform: rotate(135deg);
}
.cal {
  font-family: "Roboto", sans-serif;
  font-size: 11px;
  color: #666;
  border: 1px solid #bbb;
  width: 428px;
  box-shadow: 0px 0px 4px #999;
}
.cal-header {
  display: flex;
  height: 60px;
  justify-content: space-between;
  align-items: center;
}
.cal-header-nav {
  width: 60px;
  height: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.cal-header-title {
  font-size: 19px;
}
.cal-table {
  width: 100%;
  border-collapse: collapse;
}
.cal-table th {
  text-transform: uppercase;
  color: #aaa;
}
.cal-table thead {
  border-bottom: 1px solid #ddd;
}
.cal-table td {
  font-size: 14px;
  text-align: center;
  line-height: 14px;
  padding-top: 14px;
  padding-bottom: 14px;
}
.cal-table td.withmonth {
  padding-top: 0px;
}
.cal-table td.othermonth {
  background-color: #efefef;
}
.td-month {
  text-transform: uppercase;
  color: #ddd;
  font-size: 9px;
}
.cal-table td.othermonth .td-month {
  color: #666;
}
</style>

<template>
  <div class="Calendar">
    <div class="container">
      <div class="calendar-header">
        <a class="calendar-link" :href="getPreviousMonthPath($route.params.year, $route.params.month)">&lt; Previous</a>
        <label id="calendar-year-month">{{ $route.params.year }}/{{ $route.params.month }}</label>
        <a class="calendar-link" :href="getNextMonthPath($route.params.year, $route.params.month)">Next &gt;</a>
      </div>
      <div class="calendar-contents">
        <table class="table table-bordered">
          <thead>
            <tr>
              <td>Sun</td>
              <td>Mon</td>
              <td>Tue</td>
              <td>Wed</td>
              <td>Thu</td>
              <td>Fri</td>
              <td>Sat</td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="week in createCalendar($route.params.year, $route.params.month)" :key="week.id">
              <td v-for="item in week" :key="item.id">
                <div class="calendar-cell">
                  <span class="date-label">{{ item }}</span>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  validate({ params }) {
    return /^\d{4}$/.test(params.year) && /^\d{1,2}$/.test(params.month)
  },
  asyncData: {
    calendar: []
  },
  methods: {
    getPreviousMonthPath: (year, month) => {
      return getPathForDate(new Date(parseInt(year), parseInt(month) - 2))
    },
    getNextMonthPath: (year, month) => {
      return getPathForDate(new Date(parseInt(year), parseInt(month)))
    },
    createCalendar: (year, month) => {
      const initialYear = parseInt(year)
      const initialMonth = parseInt(month) - 1
      const initialDate = new Date(initialYear, initialMonth)
      return Array.from(Array(7).keys(), n =>
        Array.from(Array(7).keys(), day =>
          new Date(
            initialYear,
            initialMonth,
            1 - initialDate.getDay() + n * 7 + day
          ).getDate()
        )
      )
    }
  }
}

const getPathForDate = date => {
  return (
    '/calendar/' +
    date.getFullYear().toString() +
    '/' +
    (date.getMonth() + 1).toString()
  )
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Raleway:500,700');

body {
  font-family: 'Raleway', sans-serif;
}

div.calendar-header {
  display: flex;
  justify-content: space-between;
  margin: 12px 0;
  align-items: center;
}

label#calendar-year-month {
  font-size: 20px;
}

.calendar-link {
  transition: 0.8s;
  color: #202528;
  padding: 0 8px;
  border-bottom: solid 1px transparent;
}

.calendar-link:hover {
  color: #202528;
  padding: 0 8px;
  text-decoration: none;
  border-bottom: solid 1px #202528;
}

div.calendar-contents {
  display: flex;
  width: 100%;
  justify-content: center;
  max-width: 1200px;
}
</style>

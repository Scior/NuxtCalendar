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
              <td class="date-holiday">
                Sun
              </td>
              <td>Mon</td>
              <td>Tue</td>
              <td>Wed</td>
              <td>Thu</td>
              <td>Fri</td>
              <td class="date-saturday">
                Sat
              </td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="week in createCalendar($route.params.year, $route.params.month)" :key="week.id">
              <td v-for="item in week" :key="item.id" :class="item[1]">
                <div>
                  <label class="calendar-date">{{ item[0] }}</label>
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
      return Array.from(Array(6).keys(), n =>
        Array.from(Array(7).keys(), day => {
          const date = new Date(
            initialYear,
            initialMonth,
            1 - initialDate.getDay() + n * 7 + day
          )
          return [date.getDate(), getCssClassForDate(date, initialMonth)]
        })
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
const getCssClassForDate = (date, month) => {
  const defaultClass = 'cell-date'
  if (date.getMonth() !== month) return defaultClass + ' date-inactive'
  if (date.getDay() === 0) return defaultClass + ' date-holiday'
  if (date.getDay() === 6) return defaultClass + ' date-saturday'
  return defaultClass
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
  font-weight: 500;
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

label.calendar-date {
  font-size: 20px;
  font-weight: 700;
}

.cell-date {
  transition: 1s;
}

.cell-date:hover {
  background: #eaecee;
}

.date-inactive {
  background: #f4f5f5;
  color: #626466;
}

.date-saturday {
  color: #2020ca;
}

.date-holiday {
  color: #ca2020;
}
</style>

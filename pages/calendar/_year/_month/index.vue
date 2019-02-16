<template>
  <div class="Calendar">
    <div class="container">
      <div class="calendar-header">
        <a :href="getPreviousMonthPath($route.params.year, $route.params.month)">先月</a>
        <label>{{ $route.params.year }}年{{ $route.params.month }}月</label>
        <a :href="getNextMonthPath($route.params.year, $route.params.month)">来月</a>
      </div>
      <ul>
        <li v-for="item in createCalendar($route.params.year, $route.params.month)" :key="item.id">
          {{ item }}
        </li>
      </ul>
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
      const initialMonth = parseInt(month)
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
.calendar-header {
  display: flex;
  justify-content: space-between;
}
</style>

<style lang="stylus">
@require '../../styles/disney/var/color.styl';

.calendar {
  position: relative;
  background: #FFF;
  border-radius: 10px;
  overflow: hidden;
  min-height: 300px;
  background: $color-primary-ss;

  &__table {
    width: 100%;
    text-align: center;
    margin: 0px;
    border-collapse: collapse;
    border-spacing: 0;

    >thead {
      >tr {
        >th {
          color: #FFF;
          text-align: center;
          background: $color-primary;
          border-bottom: none;
          line-height: 40px;
          font-size: 14px;
          font-weight: 500;
        }
      }
    }

    tbody {
      tr {
        td {
          position: relative;
          background: $color-primary-ss;
          border: 1.5px solid #FFF;
        }
      }
    }
  }
}


.calendar-item {
  position: relative;
  height: 60px;
  overflow: hidden;
  border-radius: 3px;
  background: $color-primary-ss;

  &.is-pointer {
    cursor: pointer;

    &:hover {
      .calendar-item {
        &__day {
          color: $color-primary;
        }
      }

      background-color: rgba($color-primary-s, 0.2);
    }
  }

  .badge {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    position: absolute;
    right: 8px;
    bottom: 8px;
  }

  &.is-yellow {
    .badge {
      background-color: $color-yellow;
    }
  }

  &.is-green {
    .badge {
      background-color: $color-green;
    }
  }

  &.is-orange {
    .badge {
      background-color: $color-orange;
    }
  }

  &.is-red {
    .badge {
      background-color: $color-red;
    }
  }

  &.is-default {
    .badge {
      background-color: #CCC;
    }
  }

  &__day {
    position: absolute;
    top: 8px;
    left: 8px;
    font-size: 13px;
    color: $color-light-grey-s;
  }

  &__num {
    position: absolute;
    bottom: 8px;
    left: 8px;
    color: $color-light-grey;
    font-size: 14px;
  }
}
</style>

<template>
  <div class="calendar">
    <table class="calendar__table" cellpadding="5">
      <thead>
        <tr>
          <th :width="100/7 + '%'" v-for="week in WEEKS" class="week">{{$t('el.datepicker.weeks.' + [week])}}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in calendar" :key="index">
          <td v-for="(day, index) in item" :key="index">
            <slot v-if="day.day" :day="day.day" :data="data[day.index]"></slot>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import moment from 'moment'
import AttWaitTime from '@/components/Att/AttWaitTime'
import CalendarItem from './CalendarItem'
import { ATT_WAIT_CLASS } from '@/common/data/const'
const WEEKS = ['mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun']

export default {
  components: {
    AttWaitTime, CalendarItem
  },
  props: {
    data: Array,
    ym: String,
    mode: {
      type: String,
      default: 'waits'
    }
  },
  data() {
    return {
      day: 0,
      calendar: [],
      WEEKS
    }
  },
  mounted() {
    this.init()
  },
  watch: {
    ym() {
      this.init()
    }
  },
  methods: {
    isLeap(year) {
      let res
      return (year % 100 == 0 ? res = (year % 400 == 0 ? 1 : 0) : res = (year % 4 == 0 ? 1 : 0))
    },
    init() {
      const year = moment(this.ym, 'YYYY-MM').format('YYYY')
      const month = moment(this.ym, 'YYYY-MM').format('MM')
      const MONTH_DAYS = [31, 28 + this.isLeap(year), 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]

      const m = month - 1
      const firstDay = new Date(year, m, 1)

      let dayOfWeek = firstDay.getDay() - 1
      if (dayOfWeek === -1) dayOfWeek = 6
      const calendarCol = Math.ceil((dayOfWeek + MONTH_DAYS[m]) / 7)

      const calendar = []
      let index = -dayOfWeek
      for (let i = 0; i < calendarCol; i++) {
        calendar[i] = []
        for (let k = 0; k < 7; k++) {
          let idx = 7 * i + k
          let date = idx - dayOfWeek + 1
          let day

          if (date <= 0) {

          } else if (date > MONTH_DAYS[m]) {

          } else {
            day = idx - dayOfWeek + 1
          }

          if (day < 10) day = '0' + day
          calendar[i].push({
            day,
            index
          })
          index++
        }
      }
      this.calendar = calendar
    }
  }
}
</script>



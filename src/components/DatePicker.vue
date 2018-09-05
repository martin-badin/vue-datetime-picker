<template>
  <div class="datetime-picker__block">
    <div class="datetime-picker__picker">
      <div class="datetime-picker__months datetime-picker--clearfix">
        <button @click="onPrevMonth" class="datetime-picker__button datetime-picker__button--left">◀</button>
        <div class="datetime-picker__month">
          {{ formated.month }} {{ formated.year }}
        </div>
        <button @click="onNextMonth" class="datetime-picker__button datetime-picker__button--right">▶</button>
      </div>

      <div class="datetime-picker__days datetime-picker--clearfix">
        <div v-for="day in locale.days" :key="day" class="datetime-picker__day">
          {{ day }}
        </div>
        <div :class="{
          'datetime-picker__day': true,
          'datetime-picker__day--selected': isEqual(day, value),
          'datetime-picker__day--disabled': isWithinRange(day) === false
        }" v-for="day in days" :key="getTimestamp(day)" @click="onSelectDay(day)">
          {{ getDate(day) }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import add_hours from "date-fns/add_hours";
import add_milliseconds from "date-fns/add_milliseconds";
import add_minutes from "date-fns/add_minutes";
import add_seconds from "date-fns/add_seconds";
import each_day from "date-fns/each_day";
import end_of_month from "date-fns/end_of_month";
import format from "date-fns/format";
import get_date from "date-fns/get_date";
import get_day from "date-fns/get_day";
import get_days_in_month from "date-fns/get_days_in_month";
import get_hours from "date-fns/get_hours";
import get_milliseconds from "date-fns/get_milliseconds";
import get_minutes from "date-fns/get_minutes";
import get_month from "date-fns/get_month";
import get_seconds from "date-fns/get_seconds";
import get_time from "date-fns/get_time";
import is_equal from "date-fns/is_equal";
import is_within_range from "date-fns/is_within_range";
import set_date from "date-fns/set_date";
import set_month from "date-fns/set_month";
import start_of_month from "date-fns/start_of_month";

export default {
  name: "DatePicker",
  props: {
    value: {
      type: [String, Date],
      required: true
    }
  },
  inject: ["format", "locale"],
  computed: {
    days() {
      const prevMonth = set_month(this.value, this.selected.month - 1);
      const nextMonth = set_month(this.value, this.selected.month + 1);
      const prevMonthDays = get_days_in_month(prevMonth);

      return each_day(
        set_date(
          prevMonth,
          prevMonthDays - (get_day(start_of_month(this.value)) || 7) + 2
        ),
        set_date(
          nextMonth,
          this.locale.days.length - get_day(end_of_month(this.value))
        )
      );
    },
    selected() {
      return {
        month: get_month(this.value)
      };
    },
    formated() {
      const splited = this.format.split(" ");
      const monthFormat = splited.find(string => string.indexOf("M") === 0);
      const yearFormat = splited.find(string => string.indexOf("Y") === 0);
      return {
        month: format(this.value, monthFormat, { locale: this.locale }),
        year: format(this.value, yearFormat, { locale: this.locale })
      };
    }
  },
  methods: {
    onSelectDay(day) {
      let date = day;
      date = add_hours(date, get_hours(this.value));
      date = add_milliseconds(date, get_milliseconds(this.value));
      date = add_minutes(date, get_minutes(this.value));
      date = add_seconds(date, get_seconds(this.value));
      this.$emit("input", date);
    },
    onNextMonth() {
      this.$emit("input", set_month(this.value, this.selected.month + 1));
    },
    onPrevMonth() {
      this.$emit("input", set_month(this.value, this.selected.month - 1));
    },
    onOpenModal() {
      this.$refs.modal.openModal();
    },
    isWithinRange(date) {
      return is_within_range(
        date,
        start_of_month(this.value),
        end_of_month(this.value)
      );
    },
    getDate: get_date,
    getTimestamp: get_time,
    isEqual(date1, date2) {
      return is_equal(format(date1, "M-D-YYYY"), format(date2, "M-D-YYYY"));
    }
  }
};
</script>

<style lang="scss">
.datetime-picker {
  &__years,
  &__months {
    position: relative;
  }

  &__day,
  &__button {
    float: left;
  }

  &__year,
  &__month {
    text-align: center;
    position: absolute;
    left: 0;
    right: 0;
    top: 50%;
    transform: translate(0, -50%);
    pointer-events: none;
  }

  &__day {
    width: 36px;
    line-height: 36px;
    height: 36px;
    text-align: center;

    &:not(&--selected) {
      cursor: pointer;
    }

    &--selected {
      color: red;
    }

    &--disabled {
      color: #ccc;
    }
  }

  &__days {
    width: 36px * 7;
  }
}
</style>

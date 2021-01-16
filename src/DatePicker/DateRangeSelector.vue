<template>
  <div class="date-range-selector d-flex flex-row flex-nowrap align-stretch justify-start">
    <div class="d-inline-flex pl-3 pr-2">
      <v-icon :color="iconColor">{{ icon.mdiCalendarRangeOutline }}</v-icon>
    </div>
    <div class="d-inline-flex pl-1 pr-4 flex-column flex-grow justify-center">
      {{ getDateStart }} &mdash; {{ getDateUntil }}
      <small class="d-flex mt-n1 text-no-wrap" v-if="compare">
        Compare to: {{ getCompareStart }} &mdash; {{ getCompareUntil }}
      </small>
    </div>
  </div>
</template>

<style lang="scss" scoped>
// @import "~vuetify/src/styles/styles.sass";

.date-range-selector::v-deep {
  margin-top: 1px;
  margin-bottom: 1px;
  min-height: 34px;
  cursor: pointer;
} // .date-range-selector
</style>

<script>
import moment from "moment"
import { mdiCalendarRangeOutline } from "@mdi/js"

const DATE_FORMAT = "MMM D, YYYY"

export default {
  name: "DateSelector",
  props: ["iconColor", "dateStart", "dateUntil", "compareStart", "compareUntil", "compare"],
  inheritAttrs: false,

  data() {
    return {
      icon: {
        mdiCalendarRangeOutline,
      },
      moment,
    }
  }, // data

  mounted() {
    // this.$el.className += `  ${this.$el.getAttribute("data-class")}`
  }, // mounted

  computed: {
    // getDate* computed values get the dates formatted by DATE_FORMAT
    getDateStart() {
      let result

      if (this.dateStart) {
        result = this.moment(this.dateStart).format(DATE_FORMAT)
      } else {
        result = this.moment()
          .subtract(7, "days")
          .format(DATE_FORMAT)
      }

      return result
    }, // getDateStart

    getDateUntil() {
      let result

      if (this.dateUntil) {
        result = this.moment(this.dateUntil).format(DATE_FORMAT)
      } else {
        result = this.moment()
          .subtract(1, "day")
          .format(DATE_FORMAT)
      }

      return result
    }, // getDateUntil

    getCompareStart() {
      let result

      if (this.compareStart) {
        result = this.moment(this.compareStart).format(DATE_FORMAT)
      } else {
        result = this.moment()
          .subtract(15, "days")
          .format(DATE_FORMAT)
      }

      return result
    }, // getCompareStart

    getCompareUntil() {
      let result

      if (this.compareUntil) {
        result = this.moment(this.compareUntil).format(DATE_FORMAT)
      } else {
        result = this.moment()
          .subtract(8, "days")
          .format(DATE_FORMAT)
      }

      return result
    }, // getCompareUntil
  }, // computed
} // export
</script>

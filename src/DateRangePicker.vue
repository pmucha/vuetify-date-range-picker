<template>
  <div
    class="date-range-picker d-inline-flex flex-column justify-center"
    v-if="$props.config != undefined"
  >
    <v-card
      :class="[vCardClass, 'd-flex align-center']"
      :style="vCardStyle"
    >
      <v-overlay
        :value="dateSelectorOpen"
        @click.native="dateSelectorOpen = false"
      />

      <date-range-selector
        v-bind="$attrs"
        :date-start="dateStart"
        :date-until="dateUntil"
        :compare-start="compareStart"
        :compare-until="compareUntil"
        :compare="compare"
        @click.native="dateSelectorOpen = !dateSelectorOpen"
      />

      <div class="date-range-pickers-container" v-if="dateSelectorOpen">
        <date-range-picker-desktop
          :config="config"
          :compare-ranges="compare"
          @change="dateSelectorChanged"
          @close="dateSelectorOpen = false"
          v-if="this.$vuetify.breakpoint.mdAndUp"
        />
        <date-range-picker-tablet
          :config="config"
          :compare-ranges="compare"
          @change="dateSelectorChanged"
          @close="dateSelectorOpen = false"
          v-else-if="this.$vuetify.breakpoint.sm"
        />
        <date-range-picker-mobile
          :config="config"
          :compare-ranges="compare"
          @change="dateSelectorChanged"
          @close="dateSelectorOpen = false"
          v-else
        />
      </div>
    </v-card>
  </div>
</template>

<script>
import DateRangeSelector from "./DatePicker/DateRangeSelector.vue"
import DateRangePickerDesktop from "./DatePicker/DateRangePickerDesktop.vue"
import DateRangePickerTablet from "./DatePicker/DateRangePickerTablet.vue"
import DateRangePickerMobile from "./DatePicker/DateRangePickerMobile.vue"

export default {
  name: "DateRangePicker",

  components: {
    DateRangeSelector,
    DateRangePickerDesktop,
    DateRangePickerTablet,
    DateRangePickerMobile,
  }, // components

  inheritAttrs: true,

  props: {
    config: {
      type: Object,
      required: true,
    },
  }, // props

  data() {
    return {
      dateSelectorOpen: false,
      dateStart: null,
      dateUntil: null,
      compareStart: null,
      compareUntil: null,
      compare: false,
      vCardClass: "",
      vCardStyle: ""
    }
  }, // data

  mounted() {
    this.changeValues(this.config)

    this.vCardStyle = this.$el.getAttribute("data-style")
    // vcard-class goes not to the top element, but extends the <v-card />
    const dataClass = this.$el.getAttribute("data-class")
    this.vCardClass = dataClass
    if (dataClass.match(/(^|\s)outlined(\s|$)/) !== null) {
      this.vCardClass += " v-sheet--outlined"
    }

  }, // mounted

  methods: {
    dateSelectorChanged(newVals) {
      this.changeValues(newVals)
      this.$emit("change", newVals)
    },

    changeValues(newVals) {
      this.dateStart = newVals.dateStart
      this.dateUntil = newVals.dateUntil
      this.compareStart = newVals.compareStart
      this.compareUntil = newVals.compareUntil
      this.compare = newVals.compare
    },
  }, // methods
} // export
</script>

<style lang="scss" scoped>
.date-range-picker {
  cursor: pointer;
}

.date-range-pickers-container {
  position: fixed;
  top: 0;
  left: 0;
  padding: 0;
  margin: 0;
  z-index: 100;
  width: 100vw;
} // .date-range-pickers-container
</style>

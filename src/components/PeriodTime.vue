

<template>
  <div class="period">
    <div class="partA">
      <h5>Period {{ index }}</h5>
    </div>

    <div class="partB">
      <p>{{ formatDate(period.startDate) }}</p>
      <p>
        {{ dateDiff(period.startDate, period.endDate) }}
        <span style="font-size: 9px">seconds</span>
      </p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "PeriodTime",
  components: {},
  props: ["period", "index"],
  data() {
    return {};
  },
  methods: {
    formatDate: (date: Date) => {
      const time = new Date(date).toLocaleString("en-US", {
        hour: "numeric", // numeric, 2-digit
        minute: "numeric", // numeric, 2-digit
        second: "numeric", // numeric, 2-digit
      });

      return time;
    },
    dateDiff: (date1: string, date2: string) => {
      const date1Timestamp = new Date(date1).getTime();
      const date2Timestamp = new Date(date2).getTime();
      const diffSeconds = (date2Timestamp - date1Timestamp) / 1000;

      return diffSeconds;
    },
  },
});
</script>

<style lang="scss" scooped>
.period {
  display: flex;
  justify-content: space-between;
  // width: 100%;
  border-bottom: 1px solid #60b669;
  padding-left: 40px;
  padding-right: 40px;
  // background: red;
  height: 50px;
  align-items: center;
  .partA {
    // background-color: blue;
    align-items: center;
  }
  .partB {
    // background-color: green;
    width: 220px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  h4 {
    color: #60b669;
    font-size: 18px;
  }
  h5 {
    font-size: 15px;
    color: #a0a1a0;
  }
}
</style>

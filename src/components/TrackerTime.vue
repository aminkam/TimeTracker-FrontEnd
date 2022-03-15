

<template>
  <div class="tracker">
    <div class="head">
      <div class="date">Today, {{ getDate }}</div>
      <div class="headparametre">
        <button type="button" class="stop" @click="add()">Stop</button>
        <button
          type="button"
          class="start"
          @click="startValue = getCurrentTime()"
        >
          Start new
        </button>
        <h5>{{ formatDate(getCurrentTime()) }}</h5>
      </div>
    </div>
    <PeriodTime
      v-for="(period, index) in timeTrackers"
      v-bind:key="index"
      v-bind:period="period"
      v-bind:index="index"
    />
    <div class="total">
      Total <span id="total-time">{{ getTotal() }}</span>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import PeriodTime from "./PeriodTime.vue";
import axios from "axios";

export default defineComponent({
  name: "TrackerTime",
  components: { PeriodTime },
  data() {
    return {
      timeTrackers: [],
      startValue: null,
    };
  },
  async beforeMount() {
    this.timeTrackers = await this.getTimeTrackers();
  },
  methods: {
    getCurrentTime: () => {
      const date = new Date();

      return date;
    },
    formatDate: (date: Date) => {
      const time = date.toLocaleString("en-US", {
        hour: "numeric", // numeric, 2-digit
        minute: "numeric", // numeric, 2-digit
        second: "numeric", // numeric, 2-digit
      });

      return time;
    },
    getTimeTrackers: () => {
      return axios
        .get("http://localhost:3000/")
        .then((response) => response.data)
        .catch((err) => {
          console.log({ err });
          return [];
        });
    },
    add: function () {
      let obj = {
        startDate: this.startValue,
        endDate: this.getCurrentTime(),
      };

      axios
        .post("http://localhost:3000/", obj)
        .then((response) => console.log(response))
        .catch((err) => {
          console.log({ err });
          return [];
        });
    },
    dateDiff: (date1: string, date2: string) => {
      const date1Timestamp = new Date(date1).getTime();
      const date2Timestamp = new Date(date2).getTime();
      const diffSeconds = (date2Timestamp - date1Timestamp) / 1000;

      return diffSeconds;
    },
    getTotal: function () {
      let total = 0;

      this.timeTrackers.forEach((timeTrack: any) => {
        total += this.dateDiff(timeTrack.startDate, timeTrack.endDate);
      });

      return total;
    },
  },
  computed: {
    getDate: function () {
      const months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      const d = new Date();
      let month = months[d.getMonth()];
      return `${d.getDate()}  ${month}`;
    },
  },
});
</script>

<style lang="scss" scooped>
.tracker {
  display: flex;
  flex-direction: column;
  margin: 10% 20%;
  box-sizing: border-box;
  background-color: #fff;
  height: auto;
  padding: 5%;

  .head {
    display: flex;
    justify-content: space-between;
    align-items: center;
    // background: red;
    // margin-bottom: 50px;
    padding-bottom: 30px;
    height: 110px;
    border-bottom: 1.6px solid #60b669;
    .headparametre {
      width: 40%;
      align-items: center;
      display: flex;
      justify-content: space-around;
      width: 38%;
    }
    .date {
      color: #415c73;
      font-family: inter, sans-serif;
      font-size: 25px;
      line-height: 80px;
      font-weight: 700;
    }
    .start {
      color: #60b669;
      background: #fff;
      border-radius: 5px;
      width: 100px;
      height: 32px;
      font-size: 15px;
      border: 1px solid #60b669;
    }
    .start:hover {
      background: rgb(248, 247, 247);
      cursor: pointer;
    }
    .stop {
      color: rgb(236, 115, 115);
      background: #fff;
      border-radius: 5px;
      width: 80px;
      height: 32px;
      font-size: 15px;
      border: 1px solid rgb(236, 115, 115);
    }
    .stop:hover {
      background: rgb(248, 247, 247);
      cursor: pointer;
    }
    h5 {
      font-size: 18px;
      color: #60b669;
    }
  }
  .total {
    // width: 500px;
    margin-top: 70px;

    height: 40px;
    padding-top: 10px;
    // align-items: center;
    padding-left: 580px;
    font-size: 24px;
    font-weight: 800;
    border-bottom: 1.6px solid #ebefec;
    border-top: 1.6px solid #ebefec;
    color: #6a7480;

    #total-time {
      margin-left: 15px;
      color: #60b669;
    }
  }
}
</style>

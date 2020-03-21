<template>
  <div>
    <ul class="counts">
      <li><p class="active cases">
      Cases:
      <strong v-if="totalCases < 6">{{ totalCases }}</strong>
      <!-- after automatic testing stopped -->
      <strong v-else>{{ totalCases }}</strong>
    </p></li>
    <li><p class="active recovered">
      Recovered:
      <strong v-if="isNaN(totalRecovered)">0</strong>
      <strong v-else>{{ totalRecovered }}</strong>
    </p></li>
    <li><p class="active deaths">
      Deaths:
      <strong v-if="isNaN(totalDeaths)">0</strong>
      <strong v-else>{{ totalDeaths }}</strong>
    </p></li><br/>
    </ul>
    
    
    <!-- <div class="choose-date">
      <p>Dates:</p>
      <v-slider
        v-model="selectedDate"
        :tick-labels="sliderDatesFromatted"
        :max="sliderDates.length - 1"
        ticks="always"
      ></v-slider>
    </div> -->
    <p><br/>&nbsp;</p>
    <SvgElement :contagions="contagions" :totalcases="totalCases" />
    <Chart :chartcontagions="chartcontagions" />
    <newChart/>
  </div>
</template>

<script>
import moment from "moment";
import data from "../assets/data.js";
import SvgElement from "./SvgElement.vue";
import Chart from "./Chart.vue";
export default {
  name: "Data",
  components: {
    SvgElement,
    Chart
  },
  data() {
    return {
      disabledDates: {
        to: new Date(2020, 1, 22),
        from: new Date()
      },
      data,
      today: new Date(),
      contagions: [],
      chartcontagions: [],
      totalCases: 0,
      totalRecovered: 0,
      totalDeaths: 0,
      value: 0,
      fruits: 0,
      sliderDates: [],
      sliderDatesFromatted: [],
      maxTicks: 8,
      currentDate: 0,
      cantons: [
        { name: "Colombo", short: "Colombo" },
        { name: "Gampaha", short: "Gampaha" },
        { name: "Kalutara", short: "Kalutara" },
        { name: "Kandy", short: "Kandy" },
        { name: "Matale", short: "Matale" },
        { name: "Nuwara Eliya", short: "Nuwara Eliya" },
        { name: "Galle", short: "Galle" },
        { name: "Matara", short: "MT" },
        { name: "Hambantota", short: "Hambantota" },
        { name: "Jaffna", short: "Jaffna" },
        { name: "Kilinochchi", short: "Kilinochchi" },
        { name: "Mannar", short: "Mannar" },
        { name: "Vavuniya", short: "Vavuniya" },
        { name: "Mullaitivu", short: "Mullaitivu" },
        { name: "Batticaloa", short: "Batticaloa" },
        { name: "Ampara", short: "Ampara" },
        { name: "Trincomalee", short: "Trincomalee" },
        { name: "Kurunegala", short: "Kurunegala" },
        { name: "Puttalam", short: "Puttalam" },
        { name: "Anuradhapura", short: "Anuradhapura" },
        { name: "Polonnaruwa", short: "Polonnaruwa" },
        { name: "Badulla", short: "Badulla" },
        { name: "Moneragala", short: "Moneragala" },
        { name: "Ratnapura", short: "Ratnapura" },
        { name: "Kegalle", short: "Kegalle" },
        { name: "Quarantine-Centers", short: "Quarantine-Centers" },
        { name: "Foreigners", short: "Foreigners" }
      ]
    };
  },
  computed: {
    selectedDate: {
      get() {
        return this.currentDate;
      },
      set(value) {
        this.currentDate = value;
        this.changeDate();
      }
    }
  },
  created() {
    this.currentDate = this.maxTicks - 1;
    this.setSliderDates(new Date(2019, 12, 22), new Date(), this.maxTicks);
    this.loadData(`data_${moment(new Date()).format("MMDD")}`);
    this.loadChartData();
  },
  methods: {
    // prepare data for the Chart component
    loadChartData() {
      let day = 0;
      Object.entries(this.data.days).forEach(([key, val]) => {
        const obj = {};
        if (key === "data_0121") {
          obj["date"] = "21.01";
        } else {
          obj["date"] = `+${day}`;
        }
        day++;
        // prepare the totals
        let dayTotal = 0;
        if (key === "data_0330") {
          // dayTotal = 9;
        } else {
          val.forEach(function(element) {
            dayTotal += element["Cases"];
          });
        }
        obj["total"] = dayTotal;
        // push dates + totals
        this.chartcontagions.push(obj);
      });
    },
    loadData(x) {
      this.totalCases = 0;
      this.totalDeaths = 0;
      this.totalRecovered = 0;
      this.contagions = [];
      // process data from csvs, create an array of cases objects per canton
      if (this.data.days[x] !== undefined) {
        this.data.days[x].forEach(element => {
          const obj = {};
          obj["name"] = this.cantons.find(
            ({ name }) => name === element.Titel
          ).short;
          obj["cases"] = element.Cases;
          obj["deaths"] = element.Deaths;
          obj["recovered"] = element.Recovered;
          this.contagions.push(obj);
        });
        this.contagions.forEach(element => {
          this.totalCases += element["cases"];
          this.totalDeaths += element["deaths"];
          this.totalRecovered += element["recovered"];
        });
      } else {
        // unavailable: day not imported yet
        // => get last day with data
        let i = 0;
        Object.entries(this.data.days).forEach(([key]) => {
          const keyString = [
            key.replace("data_", "").slice(0, 2),
            key.replace("data_", "").slice(2, 4)
          ].join("");
          if (i < parseInt(keyString)) {
            i = keyString;
          }
        });
        this.loadData(`data_${i}`);
      }
    },
   
    customFormatter(date) {
      return moment(date).format("DD.MM.YYYY");
    },
    changeDate() {
      this.previousSelectedDate = this.selectedDate;
      this.loadData(
        `data_${this.sliderDates[this.selectedDate].format("MMDD")}`
      );
    },
    setSliderDates(from, to, maxSteps) {
      const fromDate = moment(from);
      const toDate = moment(to);
      const daysBetweenDays = toDate.diff(fromDate, "days");
      const perfectIncrement = Math.floor(daysBetweenDays / (maxSteps - 1));

      let day = fromDate;

      const addDay = day => {
        this.sliderDates.push(day);
        this.sliderDatesFromatted.push(day.format("DD.MM"));
      };

      for (let i = 0; i < maxSteps - 1; i++) {
        addDay(day);
        day = day.clone().add(perfectIncrement, "d");
      }

      addDay(toDate);
    }
  }
};
</script>

<style scoped lang="scss">
.active {
  margin-bottom: 0;
  margin-top: 0;
  strong {
    font-size: 1.6em;
    color: #ef233c;
  }
}
.choose-date {
  display: flex;
  justify-content: center;
  align-items: center;
  @media (max-width: 600px) {
    flex-direction: column;
    margin-top: 13px;
    > div {
      width: 100%;
    }
  }
  @media (max-width: 500px) {
    align-items: flex-start;
    > div {
      font-size: 12px;
    }
  }
}

.counts{
  list-style-type: none;
  padding-left: 0;

  li{
    display: inline-block;
    padding: 15px;
  }
}

.v-input{
  font-size:12px !important;
}
</style>

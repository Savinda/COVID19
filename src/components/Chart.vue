<template>
  <div class="chart-wrapper">
    <apexchart type="area" :options="options" :series="series"></apexchart>
  </div>
</template>

<script>
import VueApexCharts from "vue-apexcharts";
export default {
  props: ["chartcontagions"],
  data() {
    return {
      options: {
        chart: {
          id: "total-cases",
          type: "area",
          fontFamily: "Avenir, Helvetica, Arial, sans-serif",
          foreColor: "#444",
          toolbar: {
            autoSelected: "pan",
            show: false
          },
          zoom: {
            enabled: false
          }
        },
        
        // dataLabels: {
        //   enabled: false
        // },
        annotations: {
          xaxis: [
            {
              x: "+1",
              borderColor: "#ef233c",
              label: {
                borderColor: "#ef233c",
                style: {
                  color: "#fff",
                  background: "#ef233c",
                  fontWeight: 700
                },
                text: "🦠 JAN 22 - 1st case in LK (Chinese patient)"
              }
            },
            
            // {
            //   x: "+9",
            //   borderColor: "#ef233c",
            //   label: {
            //     borderColor: "#ef233c",
            //     style: {
            //       color: "#ffffff",
            //       background: "#ef233c",
            //       fontWeight: 700
            //     },
            //     text: "🚨 30 JAN - WHO declares global health emergency"
            //   }
            // },
            // {
            //   x: "+11",
            //   borderColor: "#FFBF00",
            //   label: {
            //     borderColor: "#FFBF00",
            //     style: {
            //       color: "#000000",
            //       background: "#FFBF00",
            //       fontWeight: 700
            //     },
            //     text: "🛬 1 FEB - 33 Sri Lankan students arrived from Wuhan"
            //   }
            // },
            // {
            //   x: "+24",
            //   borderColor: "#FFBF00",
            //   label: {
            //     borderColor: "#FFBF00",
            //     style: {
            //       color: "#000000",
            //       background: "#FFBF00",
            //       fontWeight: 700
            //     },
            //     text: "🚌 14 FEB - 33 Students quarantined and return home"
            //   }
            // },
            // {
            //   x: "+29",
            //   borderColor: "#7db500",
            //   label: {
            //     borderColor: "#7db500",
            //     style: {
            //       color: "#FFFFFF",
            //       background: "#7db500",
            //       fontWeight: 700
            //     },
            //     text: "🇨🇳 19 FEB - Recovered Chinese patient departed"
            //   }
            // },
            {
              x: "+48",
              borderColor: "#ef233c",
              label: {
                borderColor: "#ef233c",
                style: {
                  color: "#fff",
                  background: "#ef233c",
                  fontWeight: 700
                },
                text: "🦠 11 MAR - 1st Lankan in country"
              }
            },
          ]
        },
        colors: ["#ef233c"],
        xaxis: {
          type: "category",
          categories: [],
          title: {
            text: "Days since 22nd January"
          }
        },
        yaxis: {
          title: {
            text: "Total COVID-19 Cases in Sri Lanka"
          }
        },
        
        responsive: [
          {
            breakpoint: 600,
            options: {
              chart: {
                height: "400px"
              }
            }
          }
        ]
      },
      series: [
        {
          name: "Confirmed",
          data: []
        },
        // {
        //   name: "Recovered",
        //   data: []
        // }
      ]
    };
  },
  
  created() {
    const days = [];
    this.chartcontagions.forEach(function(element) {
      days.push((element.date));
    });
    this.options.xaxis.categories = days;
    const arrayTotals = [];
    this.chartcontagions.forEach(function(element) {
      arrayTotals.push(element.total);
    });
    this.series[0].data = arrayTotals;
    this.series[1].data = arrayTotals;
  },
  
  components: {
    apexchart: VueApexCharts
  }
};
</script>

<style lang="scss" scoped>
.chart-wrapper {
  max-width: 1200px;
  margin: 0 auto;
}
</style>

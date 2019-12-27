<template>
  <v-container
    fluid
    fill-height
  >
    <v-layout
      row
      wrap
    >
      <v-flex
        xs12
      >
        <h1>month picker</h1>
      </v-flex>
    </v-layout>
    <v-layout
      row
      wrap
    >
      <v-flex
        xs12
        sm12
        md6
        lg6
        class="preloader"
        slot="placeholder"
      >
        <loading
          :active.sync="isLoading"
          :is-full-page="fullPage"
          color="#7f0000"
          loader="dots"
        ></loading>
        <highcharts :options="chartOptions"></highcharts>
      </v-flex>
      <v-flex
        xs12
        sm12
        md6
        lg6
        class="preloader"
        slot="placeholder"
      >
        <loading
          :active.sync="isLoading1"
          :is-full-page="fullPage1"
          color="#7f0000"
          loader="dots"
        ></loading>
        <highcharts :options="chartOptions1"></highcharts>
      </v-flex>
      <v-flex
        xs12
        sm12
        md6
        lg6
      >
        <loading
          :active.sync="isLoading2"
          :is-full-page="fullPage2"
          color="#7f0000"
          loader="dots"
        ></loading>
        <highcharts :options="chartOptions2"></highcharts>
      </v-flex>
      <v-flex
        xs12
        sm12
        md6
        lg6
      >
        <loading
          :active.sync="isLoading3"
          :is-full-page="fullPage3"
          color="#7f0000"
          loader="dots"
        ></loading>
        <highcharts :options="chartOptions3"></highcharts>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import { serverBus } from "../main";

export default {
  data: () => ({
    chartOptions: {
      chart: {
        type: 'column',
        backgroundColor: '#FAFAFA',
        height: 250,
        // width: 400,
        animation: {
          duration: 1000,
          reflow: true
        }
      },
      title: {
        text: 'Top 5 Revenue',
        style: {
          color: '#000000',
          font: 'bold 20px "Trebuchet MS", Verdana, sans-serif'
        }
      },
      xAxis: {
        categories: null,
        labels: {
          style: {
            color: '#424242',
            fontSize: '13px'
          }
        }
      },
      yAxis: {
        title: {
          text: '% contrib'
        },
        gridLineWidth: 0.5,
        minorGridLineWidth: 0,
      },
      plotOptions: {
        column: {
          pointPadding: 0.3,
          borderWidth: 1,
          groupPadding: 0,
          // pointWidth: 35,
          dataLabels: {
            enabled: true
          }
        }
      },
      series: [{
        showInLegend: false,
        data: null,
        // pointWidth: 30,
        animation: false
      }]
    },
    chartOptions1: {
      chart: {
        type: 'column',
        backgroundColor: '#FAFAFA',
        height: 250,
        // width: 400,
        animation: {
          duration: 1000,
          reflow: true
        }
      },
      title: {
        text: 'Top 5 Opticals',
        style: {
          color: '#000000',
          font: 'bold 20px "Trebuchet MS", Verdana, sans-serif'
        }
      },
      xAxis: {
        categories: null,
        labels: {
          style: {
            color: '#424242',
            fontSize: '13px'
          }
        }
      },
      yAxis: {
        title: {
          text: '% contrib'
        },
        gridLineWidth: 0.5,
        minorGridLineWidth: 0,
      },
      plotOptions: {
        column: {
          pointPadding: 0.3,
          borderWidth: 1,
          groupPadding: 0,
          // pointWidth: 35,
          dataLabels: {
            enabled: true
          }
        }
      },
      series: [{
        showInLegend: false,
        data: null,
        // pointWidth: 30,
        animation: false
      }]
    },
    chartOptions2: {
      chart: {
        type: 'column',
        backgroundColor: '#FAFAFA',
        height: 250,
        // width: 400,
        animation: {
          duration: 1000,
          reflow: true
        }
      },
      title: {
        text: 'Top 5 Pharmacy',
        style: {
          color: '#000000',
          font: 'bold 20px "Trebuchet MS", Verdana, sans-serif'
        }
      },
      xAxis: {
        categories: null,
        labels: {
          style: {
            color: '#424242',
            fontSize: '13px'
          }
        }
      },
      yAxis: {
        title: {
          text: '% contrib'
        },
        gridLineWidth: 0.5,
        minorGridLineWidth: 0,
      },
      plotOptions: {
        column: {
          pointPadding: 0.3,
          borderWidth: 1,
          groupPadding: 0,
          // pointWidth: 35,
          dataLabels: {
            enabled: true
          }
        }
      },
      series: [{
        showInLegend: false,
        data: null,
        // pointWidth: 30,
        animation: false
      }]
    },
    chartOptions3: {
      chart: {
        type: 'column',
        backgroundColor: '#FAFAFA',
        height: 250,
        // width: 400,
        animation: {
          duration: 1000,
          reflow: true
        }
      },
      title: {
        text: 'Top 5 Surgery',
        style: {
          color: '#000000',
          font: 'bold 20px "Trebuchet MS", Verdana, sans-serif'
        }
      },
      xAxis: {
        categories: null,
        labels: {
          style: {
            color: '#424242',
            fontSize: '13px'
          }
        }
      },
      yAxis: {
        title: {
          text: '% contrib'
        },
        gridLineWidth: 0.5,
        minorGridLineWidth: 0,
      },
      plotOptions: {
        column: {
          pointPadding: 0.3,
          borderWidth: 1,
          groupPadding: 0,
          // pointWidth: 35,
          dataLabels: {
            enabled: true
          }
        }
      },
      series: [{
        showInLegend: false,
        data: null,
        // pointWidth: 30,
        // groupPadding: 0,
        animation: false
      }]
    },
    isLoading: true,
    isLoading1: true,
    isLoading2: true,
    isLoading3: true,
    fullPage: false,
    fullPage1: false,
    fullPage2: false,
    fullPage3: false
  }),
  created () {
    this.getChartData()
  },
  methods: {
    getChartData () {
      this.axios.get(`https://scm.dragarwal.com/chart`).then(response => {
        this.processChart(response.data)
      })
    },
    processChart (response) {
      let top5branches = [], top5branchesmtd = [], top5opticals = [], top5opticalsrev = [], top5pharmacy = [], top5pharmacyrev = [], top5surgery = [], top5surgeryrev = []
      response.top5.forEach(element => {
        top5branches.push(element.native)
        top5branchesmtd.push({ y: (Math.ceil(element.mtd / 100000)), color: '#EF6C00' })
      });
      this.chartOptions.xAxis.categories = top5branches
      this.chartOptions.series[0].data = top5branchesmtd
      this.isLoading = false
      response.top5pharma.forEach(element => {
        top5pharmacy.push(element.native)
        top5pharmacyrev.push({ y: (Math.ceil(element.pharmacy / 100000)), color: '#ab1234' })
      });
      this.chartOptions1.xAxis.categories = top5pharmacy
      this.chartOptions1.series[0].data = top5pharmacyrev
      this.isLoading1 = false
      response.top5opticals.forEach(element => {
        top5opticals.push(element.native)
        top5opticalsrev.push({ y: Math.ceil(element.opticals / 100000), color: '#78909C' })
      });
      this.chartOptions2.xAxis.categories = top5opticals
      this.chartOptions2.series[0].data = top5opticalsrev
      this.isLoading2 = false
      response.top5surgery.forEach(element => {
        top5surgery.push(element.native)
        top5surgeryrev.push({ y: (Math.ceil(element.surgery / 100000)), color: '#ll0045' })
      });
      this.chartOptions3.xAxis.categories = top5surgery
      this.chartOptions3.series[0].data = top5surgeryrev
      this.isLoading3 = false
    }
  }
};
</script>

<style>
</style>

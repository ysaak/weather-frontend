<template>
  <section>
    <div class="flex-container">
      <h1 class="item-fluid">{{ sensor.name }}</h1>

      <div>
        <img src="/img/unreachable.svg" v-if="sensor.lastData != null && !sensor.lastData.reachable" alt="Sensor unreachable" height="30px" width="30px" />
        <img src="/img/low-battery.svg" v-if="sensor.batteryWarning"  alt="Low battery" height="30px" width="30px" />
      </div>
    </div>
    <div class="grid-3 has-gutter">
      <div class="sensor-card col-2">
        {{ sensor.description }}
      </div>
      <div v-if="sensor.lastData != null" class="grid-2 has-gutter">
        <DataCard :title="'Température'" :value="sensor.lastData.temperature" :unit="'°C'" />
        <DataCard :title="'Humidité'" :value="sensor.lastData.humidity" :unit="'%'" />
        <DataCard :title="'Batterie'" :value="sensor.lastData.batteryVoltage" :unit="'V'" />
        <DataCard :title="'Niveau batterie'" :value="sensor.lastData.batteryLevel" :unit="'%'" />
      </div>
    </div>
    <div class="card">
      <LineChart :data="barChartData" :options="barChartOptions" :height="100" />
    </div>
  </section>
</template>

<script>
import LineChart from '~/components/LineChart'

export default {
  name: 'sensors-code',
  data() {
    return {
      sensor: {},
      barChartData: {},
      barChartOptions: {}
    };
  },
  mounted() {
    this.$nuxt.$emit('page-change', {
      title: this.sensor.name,
      rootPage: false
    })
  },
  async asyncData({ params, $moment }) {
    const data = await fetch(`http://localhost:8080/view/sensors/${params.code}`).then(res => res.json())
    const graphDayData = await fetch(`http://localhost:8080/view/sensors/${params.code}/graph/day`).then(res => res.json())

    var temperatureData = [];
    graphDayData.temperatureData.forEach(element => {
      temperatureData.push({
        x: $moment.unix(element.time),
        y: element.value
      })
    });

    var humidityData = [];
    graphDayData.humidityData.forEach(element => {
      humidityData.push({
        x: $moment.unix(element.time),
        y: element.value
      })
    });

    var graphData = {
      datasets: [
        {
          label: 'Temperature',
          yAxisID: 'temperature',
          borderColor: '#e53935',
          fill: false,
          data: temperatureData
        },
        {
          label: 'Humidité',
          yAxisID: 'humidity',
          borderColor: '#27a4fb',
          fill: false,
          data: humidityData
        }
      ]
    };

    var graphOptions = {
      responsive: true,
      legend: {
        labels: {
          fontColor: '#9bbcd1',
        }
      },
      scales: {
        xAxes: [
          {
						type: 'time',
						display: true,
						scaleLabel: {
							display: true,
							labelString: 'Date'
						},
						time: {
              unit: 'hour',
              displayFormats: {
                hour: 'HH:mm'
              }
            },
						ticks: {
              min: $moment.unix(graphDayData.minDate),
              max: $moment.unix(graphDayData.maxDate),
              fontColor: '#9bbcd1',
              major: {
                fontStyle: 'bold',
                fontColor: '#FF0000'
              }
						}
					}
        ],
        yAxes: [
          {
            id: 'temperature',
            type: 'linear',
            position: 'left',
            display: true,
            scaleLabel: {
              display: true,
              labelString: 'Température',
              fontColor: '#9bbcd1',
            },
            ticks: {
              min: 0,
              fontColor: '#9bbcd1',
            }
          },
          {
            id: 'humidity',
            type: 'linear',
            position: 'right',
            display: true,
            scaleLabel: {
              display: true,
              labelString: 'Humidité',
              fontColor: '#9bbcd1',
            },
            ticks: {
              min: 0,
              max: 100,
              fontColor: '#9bbcd1',
            }
          }
        ]
      }
    };

    return {
      sensor: data,
      barChartData: graphData,
      barChartOptions: graphOptions
    } 
  }
}
</script>

<style>
</style>

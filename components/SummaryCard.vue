<template>
    <div class="sensor-card">
        <nuxt-link :to="{ name: 'sensors-code', params: { code: sensor.code }}" class="card-title">{{ sensor.name }}</nuxt-link>
        <div class="card-body">
            <div class="icon-column">
                <img src="/img/unreachable.svg" v-if="sensor.unreachable" alt="Sensor unreachable" height="30px" width="30px" />
                <img src="/img/low-battery.svg" v-if="sensor.lowBattery"  alt="Low battery" height="30px" width="30px" />
            </div>
            <ul class="sensor-data" v-if="!sensor.unreachable">
                <li>Temp&eacute;rature <span>{{ sensor.temperature }}&deg;C</span></li>
                <li>Humidit&eacute; <span>{{ sensor.humidity }}%</span></li>
            </ul>
        </div>
        <div v-if="!sensor.unreachable" class="update-time">Mise &agrave; jour&nbsp;: {{ sensor.lastUpdateTime }}</div>
    </div>
</template>

<script>
export default {
  name: "summary-card",
  props: {
    sensor: Object
  }
}
</script>
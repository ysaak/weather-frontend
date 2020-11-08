<template>
  <section>
    <p style="text-align: right"><button v-on:click="refreshData">Actualiser</button></p>
    <div class="grid-4-small-2 has-gutter">
        <SummaryCard v-for="item in sensorList" v-bind:sensor="item" v-bind:key="item.code"></SummaryCard>
    </div>
  </section>
</template>

<script>
import SummaryCard from "@/components/SummaryCard.vue";
import { methods } from '~/../weather/frontend/src/components/IndexPage.vue';

export default {
  name: 'index',
  data() {
    return {
      sensorList: []
    };
  },
  components: {
    SummaryCard
  },
  mounted() {
    this.$nuxt.$emit('page-change', {
      title: 'Météo maison',
      rootPage: true
    })
  },
  async asyncData({ $axios }) {
    const data = await fetch(`http://localhost:8080/view/sensors/`).then(res => res.json())

    return {
      sensorList: data
    } 
  },
  methods: {
    refreshData() {
      console.log('Refresh data');
      var that = this;

      this.$axios.get(`http://localhost:8080/view/sensors/`).then(res => {
        that.sensorList = res.data
      })
    }
  }
}
</script>

<style>
</style>

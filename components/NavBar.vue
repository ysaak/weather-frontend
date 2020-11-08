<template>
  <nav class="topbar"> 
    <nuxt-link to="/" v-if="backLinkVisible" class="brand">&lt;</nuxt-link>
    <nuxt-link to="/" class="brand">{{ title }}</nuxt-link>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      title: 'Météo maison',
      backLinkVisible: false
    };
  },
  created() {
    var that = this;
    this.$nuxt.$on('page-change', function(data) {
      that.title = data.title;
      that.backLinkVisible = !data.rootPage;
    });
  }
}
</script>
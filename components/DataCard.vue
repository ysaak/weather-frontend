<template>
  <div class="card data-card">
    <div class="data-type">{{ title }}</div>
    <div class="data-value">{{ value }} {{ unit }}</div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    },
    value: {
      type: Number,
      required: true
    },
    unit: {
      type: String,
      required: true
    }
  }
}
</script>>

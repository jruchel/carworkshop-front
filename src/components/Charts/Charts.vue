<template>
  <div>
    <h2>Statystyki</h2>
    <Chart endpoint="/data/orders"></Chart>
    <Chart endpoint="/data/opinions"></Chart>
  </div>
</template>

<script>
import Chart from "@/components/Charts/Chart"
export default {
  name: "Charts",
  components: {Chart}
}
</script>

<style scoped>

</style>
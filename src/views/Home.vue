<template>
  <div class="home">
    <echarts></echarts>
  </div>
</template>

<script>
import echarts from "../components/echarts";
export default {
  name: "Home",
  components: {
    echarts
  }
};
</script>
<style>
.home {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>

<template>
  <div id="app">
    <div class="chart-container">
      <chart height="100%" width="100%" title="Temperatura w Bielsku-Białej" :bgcolor="colors.bg" :contentcolor="colors.content"/>
    </div>
    <p></p>
  </div>
</template>

<script>
import Chart from '@/components/lineMarker'

export default {
  name: 'app',
  components: {
    Chart
  },
  data () {
    return {
      colors: {
        bg: '#202336',
        content: '#C2EFB3'
      }
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background-color: #202336;
  font-family: Montserrat, sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  height: 100vh;
  width: 100%;
}

#app p {
  padding-top:20px;
  text-align: center;
  flex: 1;
  color: #C2EFB3;
  font-size: 2.0em;
  width: 100%;
}

.chart-container{
  flex: 1;
  width: 100%;
  height: calc(100vh - 84px);
}
</style>

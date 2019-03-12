<template>
  <div>
    <TsdbLine />
    <TsdbGauge />
  </div>
</template>

<script>
import TsdbLine from './TsdbLine.vue'
import TsdbGauge from './TsdbGauge.vue'

export default {
  name: 'TsdbChart',
  components: {
    TsdbLine,
    TsdbGauge
  },
  data () {
    return {
    }
  }
}
</script>

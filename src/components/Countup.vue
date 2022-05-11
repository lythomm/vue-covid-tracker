<template>
  <div class="iCountUp">
    <!-- <ICountUp
        :startVal="startVal"
        :endVal="endVal"
        :decimals="decimals"
        :duration="duration"
        :options="options"
        @ready="onReady"
    /> -->
    <div id="counter">0</div>
  </div>
</template>

<script>
import ICountUp from 'vue-countup-v2';
import { CountUp } from 'countup.js'

export default {
    name: 'Countup',
    components: {
        ICountUp,
        CountUp
    },
    data() {
        return {
            startVal: 0,
            endVal: 120500,
            decimals: 0,
            duration: 2.5,
            options: {
                useEasing: true,
                useGrouping: true,
                separator: ',',
                decimal: '.',
                prefix: '',
                suffix: ''
            }
        };
    },
    methods: {
        onReady: function(instance, CountUp) {
            const that = this;
            instance.update(that.endVal + 100);
        },
        startCount () {
            let countUp = new CountUp('counter', 89451);
            countUp.start();
        }
    },
};
</script>

<style scoped>
  .iCountUp {
      font-size: 12em;
      margin: 0;
      color: #4d63bc;
  }
</style>
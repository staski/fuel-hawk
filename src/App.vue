<template>
 <div>
<b-navbar toggleable="sm" variant="dark" type="dark">
    <b-navbar-brand href="#" class="mr-5">
        DEEBU FuelHawk
    </b-navbar-brand>
</b-navbar>

<b-container>
  <b-row>
    <b-col>
      <fuel-tank-input name="Left Tip" tankType="Tip" :dipValue="leftTipDipStick" :litersPerHour="litersPerHour" v-model="leftTipVolume"></fuel-tank-input>
    </b-col>
    <b-col>
      <fuel-tank-input name="Left Main" tankType="Main" :dipValue="leftMainDipStick" :litersPerHour="litersPerHour" v-model="leftMainVolume"></fuel-tank-input>
    </b-col>
    <b-col>
      <fuel-tank-input name="Right Main" tankType="Main" :dipValue="rightMainDipStick" :litersPerHour="litersPerHour" v-model="rightMainVolume"></fuel-tank-input>
    </b-col>
    <b-col>
      <fuel-tank-input name="Right Tip" tankType="Tip" :dipValue="rightTipDipStick" :litersPerHour="litersPerHour" v-model="rightTipVolume"></fuel-tank-input>
    </b-col>
  </b-row>
  <b-row>
    <b-col sm="auto"  >
      <b-form-group
        id="fuel-group"
        label="Total Fuel:"
        label-for="fuel-group"
      >
        <b-form-input
          size="sm"
          id="input-fuel"
          v-model="totalVolume"
          type="text"
          required
          disabled
        ></b-form-input>
      </b-form-group>
      <b-form-group
        id="endurance-group"
        label="Endurance:"
        label-for="endurance-group"
      >
        <b-form-input
          size="sm"
          id="input-endurance"
          v-model="endurance"
          type="text"
          required
          disabled
          trim
        ></b-form-input>
      </b-form-group>
    </b-col>
  </b-row>
</b-container>

</div>
</template>

<script>
import Vue from 'vue';
import { BootstrapVue, BootstrapVueIcons } from 'bootstrap-vue'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
Vue.use(BootstrapVue)
Vue.use(BootstrapVueIcons)

import FuelTankValues from './components/FuelTankValues.vue'
Vue.component('fuel-tank-input', FuelTankValues);
export default {
  name: 'app',
  mounted: function () {
    this.leftTipDipStick = 12
  },

  data: function () {
    return {
          litersPerHour : 50,
          leftTipDipStick : 12,
          leftMainDipStick : 12,
          rightMainDipStick : 12,
          rightTipDipStick : 12,
          leftTipVolume : 0,
          leftMainVolume : 0,
          rightMainVolume : 0,
          rightTipVolume : 0,
      }
    },

    computed : {
      ltEndurance : function () {
        return this.enduranceForVolume(this.tipVolumeForDipStick(this.leftTipDipStick))
      },

      ltVolume : function () {
        return this.tipVolumeForDipStick(this.leftTipDipStick) + " Liters"
      },
      totalVolume : function () {
        return this.leftTipVolume + this.leftMainVolume + this.rightMainVolume + this.rightTipVolume
      },
      endurance : function () {
        return this.enduranceForVolume(this.totalVolume)
      }

    },
    methods: {
      showAdvice : function (test){
        this.totalVolume = test
      },
      getMainTanksVolume () {
      },

      getTotalVolume () {
        return this.totalVolume
      },
      enduranceForVolume (volume) {
        var litersPerMinute = this.litersPerHour / 60
        var minutes = volume / litersPerMinute
        var hours = Math.floor(minutes/60)
        minutes -= hours * 60
        minutes = Math.round(minutes)

        return (hours < 10 ? "0" : "") + hours + ":" + (minutes < 10 ? "0" : "") + minutes
      },

    }, 
  }
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<template>
 <div>
<b-navbar toggleable="sm" variant="dark" type="dark">
    <b-navbar-brand href="#" class="mr-5">
        DEEBU FuelHawk
    </b-navbar-brand>
    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav class="ml-auto">
          <b-btn @click="configVisible = !configVisible"
              :class="configVisible ? null : 'collapsed'"
              class="mx-1">
              <b-icon icon="gear-fill" aria-hidden="true"></b-icon>
              Settings
          </b-btn>
        </b-navbar-nav>
      </b-collapse>
</b-navbar>
<div align="center" class="mb-3">
 <b-collapse id="collapse-config" v-model="configVisible">
   <b-card class="m-2" bg-variant="light" style="max-width: 20rem;">
    <label for="fuel-setting">Fuel consumption </label>
        <b-form-spinbutton id="fuel-setting" v-model="litersPerHour" min="40" max="60" 
        :formatter-fn="consumptionString"
        number
        inline
        class="mx-1"
        ></b-form-spinbutton>
    </b-card>
</b-collapse>
</div>
<b-container>
  <b-row>
    <b-col>
      <fuel-tank-input name="Left Tip Dipstick reading" tankType="Tip" :dipValue="leftTipDipStick" :litersPerHour="litersPerHour" v-model="leftTipVolume"></fuel-tank-input>
    </b-col>
    <b-col>
      <fuel-tank-input name="Left Main Dipstick reading" tankType="Main" :dipValue="leftMainDipStick" :litersPerHour="litersPerHour" v-model="leftMainVolume"></fuel-tank-input>
    </b-col>
</b-row>
<b-row>
    <b-col>
      <fuel-tank-input name="Right Main Dipstick reading" tankType="Main" :dipValue="rightMainDipStick" :litersPerHour="litersPerHour" v-model="rightMainVolume"></fuel-tank-input>
    </b-col>
    <b-col>
      <fuel-tank-input name="Right Tip Dipstick reading" tankType="Tip" :dipValue="rightTipDipStick" :litersPerHour="litersPerHour" v-model="rightTipVolume"></fuel-tank-input>
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
          v-model="totalVolumeString"
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

import FuelTankInputSB from './components/FuelTankInputSB.vue'
Vue.component('fuel-tank-input', FuelTankInputSB);
export default {
  name: 'app',
  data: function () {
    return {
          configVisible : false,
          litersPerHour : 50,
          leftTipDipStick : 12,
          leftMainDipStick : 11.5,
          rightMainDipStick : 11.5,
          rightTipDipStick : 12,
          leftTipVolume : 0,
          leftMainVolume : 0,
          rightMainVolume : 0,
          rightTipVolume : 0,
      }
    },

    computed : {
      totalVolume : function () {
        return this.leftTipVolume + this.leftMainVolume + this.rightMainVolume + this.rightTipVolume
      },
      totalVolumeString : function () {
        var v = this.totalVolume
        return v + " Liters"
      },
      endurance : function () {
        return this.enduranceForVolumeString(this.totalVolume)
      }

    },
    methods: {
      enduranceForVolume (volume) {
        var litersPerMinute = this.litersPerHour / 60
        var minutes = volume / litersPerMinute
        var hours = Math.floor(minutes/60)
        minutes -= hours * 60
        minutes = Math.round(minutes)

        return (hours < 10 ? "0" : "") + hours + ":" + (minutes < 10 ? "0" : "") + minutes
      },
      consumptionString(value){
        return value + " lph"
      },
      enduranceForVolumeString (volume){
        return this.enduranceForVolume(volume) + " @" + this.litersPerHour + " l/h"
      }   
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

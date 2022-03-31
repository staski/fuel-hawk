<template>
    <div>
        <label for="fuel-tip-sb">{{name}}</label>
        <b-form-spinbutton id="fuel-tip-sb" v-model="lDipValue" min="0" max="12" step="0.5"
        v-bind:value="value" v-on:input="$emit('input', volumeForDip)"
        number
        ></b-form-spinbutton>
        <p>Available Fuel: {{ volumeForDipStr }},<br> 
        Endurance: {{ endurance }}</p>
    </div>
</template>

<script>

export default {
    mounted : function () {
      this.$emit('input', this.volumeForDip)
    },
    name: 'Fuel-Tank-Input-SB',
    props: {
        value : Number,
        name : String,
        dipValue : Number,
        tankType : String,
        litersPerHour : Number,
    },

    computed: {
        volumeForDip : function () {
            if (this.tankType == "Tip"){
                return this.tipVolumeForDipStick(this.lDipValue)
            }
            else if (this.tankType == "Main"){
                return this.mainVolumeForDipStick(this.lDipValue)
            }
            return -1
        },
        volumeForDipStr : function () {
          return this.volumeForDip + " Liters"
        },
        endurance : function () {
            return this.enduranceForVolume(this.volumeForDip)
        }
    },

    data() {
      return {
          lDipValue : this.dipValue,
          mainDipValues : [],
          tipDipValues : []
        }
    },
    created: function () {
        this.mainDipValues = [
            [0,12],
            [1,19],
            [2,26],
            [3,33],
            [4,41],
            [5,48],
            [6,54],
            [7,62],
            [8,69],
            [9,76],
            [10,82],
            [11,90],
            [12,98],
        ],
        this.tipDipValues = [
            [2,12],
            [3,18],
            [4,22],
            [5,26],
            [6,31],
            [7,37],
            [8,42],
            [9,48],
            [10,51],
            [11,58],
            [12,61]
        ]
    },
    methods : {
      mainVolumeForDipStick (dipstickValue) {
        var prevkey = -1; 
        var prevvalue = 0;
        const count = this.mainDipValues.length

        if (dipstickValue < 0) {
          return 0
        }

        if (dipstickValue >= count) {
          return this.mainDipValues[count-1][1]
        }

        for (var i = 0; i < count; i++) {
            var key = this.mainDipValues[i][0]                          
            var value = this.mainDipValues[i][1]              
            if (key >= dipstickValue) {
              return prevvalue + (dipstickValue - prevkey)*(value - prevvalue)/(key - prevkey)
            }
            prevkey = key; prevvalue = value  
        }
      },

    tipVolumeForDipStick (dipstickValue) {
        var prevkey = -1; 
        var prevvalue = 0;
        const count = this.mainDipValues.length

        if (dipstickValue < 0) {
          return 0
        }

        if (dipstickValue >= count) {
          return this.tipDipValues[count-1][1]
        }

        for (var i = 0; i < count; i++) {
            var key = this.tipDipValues[i][0]                          
            var value = this.tipDipValues[i][1]              
            if (key >= dipstickValue) {
              return prevvalue + (dipstickValue - prevkey)*(value - prevvalue)/(key - prevkey)
            }
            prevkey = key; prevvalue = value  
        }
    },  
    enduranceForVolume (volume) {
        var litersPerMinute = this.litersPerHour / 60
        var minutes = volume / litersPerMinute
        var hours = Math.floor(minutes/60)
        minutes -= hours * 60
        minutes = Math.round(minutes)

        return (hours < 10 ? "0" : "") + hours + ":" + (minutes < 10 ? "0" : "") + minutes
    },
  }
}
</script>

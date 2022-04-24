<template>
    <div>
        <label for="fuel-tip-sb">{{name}}</label>
        <b-form-spinbutton id="fuel-tip-sb" v-model="lDipValue" min="0" :max=maxDipValue step="0.5"
        v-bind:value="value" v-on:input="$emit('input', volumeForDip)"
        number
        ></b-form-spinbutton>
        <p>Fuel: {{ volumeForDipStr }},<br> 
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
        dipStickType : String, 
    },

    computed: {
        maxDipValue : function () {
          var valuesArray = this.mainDipValues

          if (this.tankType == "Tip") {
          valuesArray = this.tipDipValues
            if (this.dipStickType == "FuelFinger"){
              valuesArray = this.tipDipValuesFuelFinger
            }
          } else {
            valuesArray = this.mainDipValues
            if (this.dipStickType == "FuelFinger") {
              valuesArray = this.mainDipValuesFuelFinger
            }
          }

          const count = valuesArray.length
          return valuesArray[count-1][0]
        },
        volumeForDip : function () {
            return this.volumeForDipStick(this.lDipValue)
        },
        volumeForDipStr : function () {
          return Math.round(this.volumeForDip*2)/2 + " Liters"
        },
        endurance : function () {
            return this.enduranceForVolume(this.volumeForDip)
        }
    },

    data() {
      return {
          lDipValue : this.dipValue
        }
    },
    created: function () {
        this.mainDipValues = [
            [0,12],
            [1.56,24],
            [3.46,36],
            [5.19,48],
            [7.13,60],
            [8.79,72],
            [10.89,84],
            [12.18,96],
        ],
        this.mainDipValuesFuelFinger = [
            [0,12],
            [2.25,24],
            [5,36],
            [7.5,48],
            [10.3,60],
            [12.7,72],
            [15.73,84],
            [17.6,96],
        ],
        this.tipDipValues = [
            [0,0],
            [1.56,12],
            [3.94,24],
            [6.35,36],
            [8.91,49],
            [11.47,60],
            [12.18,63]
        ],
        this.tipDipValuesFuelFinger = [
            [0,0],
            [2.25,12],
            [5.7,24],
            [9.17,36],
            [12.87,49],
            [16.57,60],
            [17.6,63]
        ]
    },
    methods : {
      volumeForDipStick (dipstickValue) {
        var prevkey = -1; 
        var prevvalue = 0;
        var valuesArray = this.mainDipValues

       if (this.tankType == "Tip") {
          valuesArray = this.tipDipValues
          if (this.dipStickType == "FuelFinger"){
              valuesArray = this.tipDipValuesFuelFinger
          }
       } else {
          valuesArray = this.mainDipValues
          if (this.dipStickType == "FuelFinger") {
            valuesArray = this.mainDipValuesFuelFinger
          }
       }

        const count = valuesArray.length

        if (dipstickValue < 0) {
          return 0
        }

        if (dipstickValue >= valuesArray[count-1][0]) {
          return valuesArray[count-1][1]
        }

        for (var i = 0; i < count; i++) {
            var key = valuesArray[i][0]                          
            var value = valuesArray[i][1]              
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

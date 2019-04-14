<template>
  <div>
    <input v-model="value.identifier">
    <button @click="lookup">SIMBAD lookup</button>
    RA=<input v-model.number="value.ra">, Dec=<input v-model.number="value.dec">
  </div>
</template>
<script>
import axios from 'axios';

export default {
  props: ["id", "value"],
  methods: {
    lookup: function () {
      let that = this;
      axios.get(`http://simbad.u-strasbg.fr/simbad/sim-id?Ident=${encodeURIComponent(this.value.identifier)}&output.format=ascii`).then(function (response) {
        let stringParts = response.data.match(/Coordinates\(ICRS,ep=J2000,eq=2000\): (\d+) (\d+) ([\d\.]+)\s+([\+\d]+) (\d+) ([\d\.]+)/);
        let parts = stringParts.slice(1, stringParts.length).map(Number);
        let decimalRA = (parts[0] / 24) * 360 + (parts[1] / (24 * 60)) * 360 + (parts[2] / (24 * 60 * 60)) * 360;
        let decimalDec = parts[3] + (parts[4] / 60) + (parts[5] / (60 * 60));
        console.log(`RA=${decimalRA}, Dec=${decimalDec}`);
        let value = {identifier: that.value.identifier, ra: decimalRA, dec: decimalDec};
        that.$emit('input', value);
      })
    }
  }
}
</script>
<template>
    <div>
      <div id="observatory">
      <h1>
        Observatory
      </h1>
      <div>
        <button v-for="preset in presets" @click="$emit('apply-preset', preset.payload)">{{ preset.name }}</button>
      </div>
      <ul>
        <li><label>Latitude</label><input v-model.number="observatory.latitude"></li>
        <li><label>Longitude</label><input v-model.number="observatory.longitude"></li>
        <li><label>Time zone (IANA)</label><time-zone-selector v-model="observatory.timeZone"></time-zone-selector></li>
      </ul>
      </div>
      <div id="observation">
      <h1>
        Observation
      </h1>
      <ul>
        <li><label>Local date at nightfall</label><date-entry v-model="observation.date" v-bind:timezone="observatory.timeZone"></date-entry></li>
        <li><label>Coronagraph style</label><select><option>gvAPP 180</option><option>gvAPP 360</option></select></li>
        <li><label>Inner working angle</label><div><input> &lambda; / D</div></li>
        <li><label>Outer working angle</label><div><input> &lambda; / D</div></li>
      </ul>
      </div>
  <div id="target">
    <h1>
    Target
    </h1>
    <simbad-target v-model="target"></simbad-target>
  </div>
  <div id="companions">
    <h1>
    Companions
    </h1>
    <companion-list v-bind:companions="companions"></companion-list>
  </div>
    </div>
</template>
<script>
import DateEntry from './DateEntry.vue';
import TimeZoneSelector from './TimeZoneSelector.vue';
import SimbadTarget from './SimbadTarget.vue';
import CompanionList from './CompanionList.vue';

export default {
  components: {
    DateEntry,
    TimeZoneSelector,
    SimbadTarget,
    CompanionList
  },
  props: ["observation", "observatory", "target", "companions"],
  data: function () {
    return {
      presets: [
        {name: "Magellan", payload: {observatory: {latitude: -29.0146, longitude: -70.6926, timeZone: "America/Santiago"}}},
        {name: "Subaru", payload: {observatory: {latitude: 19.825556, longitude: -155.476111, timeZone: "Pacific/Honolulu"}}},
        {name: "LBT", payload: {observatory: {latitude: 32.7016, longitude: -109.8719, timeZone: "America/Phoenix"}}}
      ]
    }
  }
}
</script>

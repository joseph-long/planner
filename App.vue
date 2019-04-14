<template>
  <div id="main">
    <div id="preview">
     <div id="plot"></div>
     <button @click="foo">Foo!</button>
    </div>
    <div id="controls">
      <controls
        v-bind:state="state"
        @apply-preset="applyPreset"
        @append-companion="appendCompanion"
        @remove-companion="removeCompanion"></controls>
      <pre>{{ state }}</pre>
    </div>
  </div>
</template>
<style lang="css">
#preview, #controls {
  padding: 1rem;
}

#preview {
  flex: 1;
  background-color: gray;
}

#controls {
  min-width: 15rem;
  overflow: scroll;
  font-size: 90%;
}
#main {
  display: flex;
  width: 100vw;
  height: 100vh;
}

input {
  font-size: inherit;
}
ul { padding: 0}
li {
  display: flex;
}
li > * {
  width: 50%;
}
</style>

<script>
import Controls from "./Controls.vue"
import Plotly from 'plotly.js-dist';
import { DateTime } from 'luxon';
import Vue from 'vue';

Vue.use((Vue) => {
  Vue.prototype.$bubble = function $bubble(eventName, ...args) {
    // Emit the event on all parent components
    let component = this;
    do {
      component.$emit(eventName, ...args);
      component = component.$parent;
    } while (component);
  };
});

function foo() {
  let TESTER = document.getElementById('plot');
  Plotly.plot( TESTER, [{
  x: [1, 2, 3, 4, 5],
  y: [1, 2, 4, 8, 16] }], {
  margin: { t: 0 } } );
}

function patchState(state, statePatch) {
  for (var key in statePatch) {
    if (typeof statePatch[key] === "object" && Object.keys(statePatch[key]).length !== 0) {
      patchState(state[key], statePatch[key])
    } else {
      state[key] = statePatch[key];
    }
  }
}

export default {
  components: {
    Controls
  },
  methods: {
    foo,
    applyPreset: function ($event) {
      patchState(this.state, $event)
    },
    appendCompanion: function ($event) {
      console.log("Add companion...");
      this.state.companions.push({separation: 1, pa: 0});
    },
    removeCompanion: function ($event, index) {
      console.log("Remove companion at...", index);
      this.state.companions.splice(index, 1);
    }
  },
  data: function () {
    return {
      state: {
        observatory: {
          latitude: 0,
          longitude: 0,
          timeZone: 'America/Santiago'
        },
        observation: {
          date: DateTime.local().toUTC(),
          coronagraph: {
            name: "gvAPP 180",
            iwa: 2,
            owa: 7,
            offset: 1.793212
          }
        },
        target: {
          identifier: "",
          ra: 0,
          dec: 0
        },
        companions: []
      }
    }
  }
}
</script>
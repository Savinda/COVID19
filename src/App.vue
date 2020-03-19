<template>
  <v-app id="app">
    <h1>COVID-19 in Sri Lanka</h1>
    <p>
      <strong>Starting from 21st January</strong><br />Detecting
      the real number of cases in Sri Lanka.
    </p>
    <Data />
    <p>Data source: Johns Hopkins University. Numbers of cases may not be exhaustive as no reliable local source for statistics.
      </p>
    <h2>Comparing situation with the World</h2>
    <p>
      in progress...
    </p>
    
    <footer>
      <p>Follow @savinda on <a href="https://github.com/Savinda" target="_blank">GitHub</a></p>
    </footer>
  </v-app>
</template>

<script>

import Data from "./components/Data.vue";

import moment from "moment";
export default {
  name: "App",
  data() {
    return {
      timestamp: null
    };
  },
  components: {
    Data
  },
  mounted() {
    this.timestamp = moment(
      document.documentElement.dataset.buildTimestampUtc
    ).fromNow();
  }
};
</script>

<style lang="scss">
html {
  height: 100%;
}
body {
  background-color: #edf2f4;
  min-height: 100%;
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2b2d42;
  padding: 40px 25% 10px 25%;
  text-align: center;
  @media (max-width: 1300px) {
    padding: 40px 15% 10px 15%;
  }
  @media (max-width: 900px) {
    padding: 40px 5% 10px 5%;
  }
  @media (max-width: 500px) {
    padding: 30px 10px 10px 10px;
    text-align: center;
  }
}
h1 {
  font-size: 1.7em;
  margin-top: 0;
  @media (max-width: 500px) {
    font-size: 1.5em;
  }
}
h2 {
  margin-top: 3rem;
  margin-bottom: 0;
  text-decoration: underline;
  @media (max-width: 500px) {
    font-size: 1.2em;
  }
}
.separator {
  height: 2px;
  background-color: rgba(43, 45, 66, 0.4);
  margin: 2em auto 2em auto;
}
.red {
  color: #ef233c;
}
.pulse {
  fill: #ef233c;
  animation-duration: 1s;
  animation-name: pulse;
  animation-iteration-count: infinite;
  transform-box: fill-box;
  transform-origin: center;
}
.skull {
  pointer-events: none;
}
@for $i from 1 through 40 {
  .pulse:nth-child(#{$i}n) {
    animation-delay: #{$i * 0.1}s;
  }
}
@keyframes pulse {
  from {
    transform: scale(1);
    opacity: 1;
  }
  to {
    transform: scale(1.7);
    opacity: 0;
  }
}
footer {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  text-align: right;
  p {
    margin: 0;
    i {
      font-size: 12px;
    }
  }
  ul {
    margin-top: 0;
    padding: 0;
    list-style-type: none;
    line-height: 14px;
    a {
      color: #ef233c;
      font-size: 12px;
    }
  }
  @media (max-width: 500px) {
    text-align: center;
    align-items: center;
  }
}
.svg-wrapper {
  position: relative;
}
.death-div {
  width: 20px;
  height: 20px;
  position: absolute;
}

.tooltip .tooltip-inner {
  box-shadow: 0px 2px 5px #2b2d42;
  background: #f9f9f9;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2b2d42;
  border-radius: 6px;
  padding: 5px 10px 4px;
}

.tooltip .tooltip-arrow {
  width: 0;
  height: 0;
  border-style: solid;
  position: absolute;
  margin: 5px;
  border-color: #f9f9f9;
  z-index: 1;
}
.tooltip[x-placement^="top"] {
  margin-bottom: 5px;
}
.tooltip[x-placement^="top"] .tooltip-arrow {
  border-width: 5px 5px 0 5px;
  border-left-color: transparent !important;
  border-right-color: transparent !important;
  border-bottom-color: transparent !important;
  bottom: -5px;
  left: calc(50% - 5px);
  margin-top: 0;
  margin-bottom: 0;
}
.tooltip.popover .popover-inner {
  background: #f9f9f9;
  color: black;
  padding: 24px;
  border-radius: 5px;
  box-shadow: 0 5px 30px rgba(black, 0.1);
}

.tooltip.popover .popover-arrow {
  border-color: #f9f9f9;
}

.tooltip[aria-hidden="true"] {
  visibility: hidden;
  opacity: 0;
  transition: opacity 0.15s, visibility 0.15s;
}

.tooltip[aria-hidden="false"] {
  visibility: visible;
  opacity: 1;
  transition: opacity 0.15s;
}

footer{
  text-align: center;
}
</style>

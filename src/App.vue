<template>
  <!-- <Selections></Selections> -->
  <!-- <Datajoins></Datajoins> -->
  <!-- <Enterexit></Enterexit> -->
  <!-- <Scales></Scales> -->
  <Shapes></Shapes>
</template>

<script>
// import chartData from "./assets/chart.json";
// import Selections from "./components/Selections.vue";
// import Datajoins from "./components/Datajoins.vue";
// import Enterexit from "./components/Enterexit.vue";
// import Scales from "./components/Scales.vue"
import Shapes from "./components/Shapes.vue"

export default {
  name: "App",
  data() {
    return {
      chart: null,
      chartData: [],
    };
  },
  components: {
    // Selections,
    // Datajoins,
    // Enterexit,
    // Scales,
    Shapes
  },
  mounted() {

  },
  methods: {
 
  },
};
</script>

<style lang="scss">
* {
  -webkit-box-sizing: border-box;
     -moz-box-sizing: border-box;
          box-sizing: border-box;
}
#app {
  
}
</style>

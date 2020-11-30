<template>
  <!-- <Selections></Selections> -->
  <!-- <Datajoins></Datajoins> -->
  <Enterexit></Enterexit>
</template>

<script>
// import chartData from "./assets/chart.json";
// import Selections from "./components/Selections.vue";
// import Datajoins from "./components/Datajoins.vue";
import Enterexit from "./components/Enterexit.vue";

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
    Enterexit
  },
  mounted() {

  },
  methods: {
 
  },
};
</script>

<style lang="scss">
#app {
  
}
</style>

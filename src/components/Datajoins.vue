<template>
  <div id="Datajoins">
    <div class="containter">
      <h1>Chart 1</h1>
      <svg id="chart-1" width="760" height="140">
        <g transform="translate(70, 70)">
          <circle r="40" />
          <circle r="40" cx="120" />
          <circle r="40" cx="240" />
          <circle r="40" cx="360" />
          <circle r="40" cx="480" />
        </g>
      </svg>
    </div>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  name: "Datajoins",
  data() {
    return {
      chart: null,
      chartData: [],
    };
  },
  components: {},
  mounted() {
    this.initChart1();
  },
  methods: {
    initChart1() {
      var scores = [
        {
          name: "Andy",
          score: 25,
        },
        {
          name: "Beth",
          score: 39,
        },
        {
          name: "Craig",
          score: 42,
        },
        {
          name: "Diane",
          score: 35,
        },
        {
          name: "Evelyn",
          score: 48,
        },
      ];

      d3.select("#chart-1")
        .selectAll("circle")
        .data(scores)
        .attr("r", function (d) {
          return d.score;
        });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  lang="scss" scoped>
#chart-1 {
    circle {
    fill: #ddd;
}
}

</style>

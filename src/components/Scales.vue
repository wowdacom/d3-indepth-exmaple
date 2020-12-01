<template>
  <div>
    <svg width="700" height="100">
      <g class="axis" transform="translate(20, 99)">
      </g>
    </svg>
  </div>
</template>

<script>
import * as d3 from "d3";
// import chartdata from "../assets/chart.json";

export default {
  name: "Scales",
  data() {
    return {};
  },
  components: {},
  mounted() {
    // console.log(d3.extent([3, 2, 1, 1, 8, 2, 4]))
    // let priceChg = chartdata.webCaseHistoryPrice.map((item) => item.priceChg);
    // let priceChgDate = chartdata.webCaseHistoryPrice.map((item) =>
    //   new Date(item.priceChgDate).getTime()
    // );

    // let max = (this.getDigits(d3.max(priceChg)) + 1) * 1000;
    // let min = (this.getDigits(d3.min(priceChg)) - 1) * 1000;
    // let dateMax = d3.max(priceChgDate);
    // let dateMin = d3.min(priceChgDate);
    // let myScaleY = d3.scaleLinear().domain([0, 100]).range([min, max]);
    // d3.scaleTime().domain([dateMin, dateMax]);

    // let axis = d3.axisLeft(myScaleY);
    // d3.select(".axis").call(axis);
    var data = [0.243, 0.584, 0.987, 0.153, 0.433];
    var extent = d3.extent(data);

    var linearScale = d3.scaleLinear()
      .domain(extent)
      .range([0, 600])
      .nice();

    var axis = d3.axisTop(linearScale);

    d3.select('.axis')
      .call(axis);
  },
  methods: {
    getDigits(myNumber) {
      let counter = 0;
      let i = Math.abs(myNumber);
      while (i >= 10) {
        i /= 10;
        counter += 1;
      }
      // console.log(i);
      // console.log(counter)
      if (counter < 3) {
        return 1;
      }
      return Math.floor(i);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  lang="scss" scoped>
body {
  font-family: "Helvetica Neue", Helvetica, sans-serif;
  font-size: 12px;
}
</style>

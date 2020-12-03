<template>
  <div class="container">
    <h1>Chart 1</h1>
    <svg id="scale1"></svg>
  </div>
  <div class="container">
    <h1>Chart 2</h1>
    <svg id="scale2"></svg>
  </div>
  <div class="container">
    <h1>Chart 3</h1>
    <svg id="scale3"></svg>
  </div>
  <div class="container">
    <h1>Chart 4</h1>
    <svg id="scale4"></svg>
  </div>
  <div class="container">
    <h1>Chart 5</h1>
    <svg id="scale5"></svg>
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
    this.handleScale1();
    this.handleScale2();
    this.handleScale3();
    this.handleScale4();
    this.handleScale5();
  },
  methods: {
    handleScale1() {
      let data = [0, 2, 3, 5, 7.5, 9, 10];
      let padding = {
        left: 10,
        right: 10,
        top: 10,
        bottom: 10,
      };
      let width = 700;
      let height = 40;
      let scale = d3
        .scaleLinear()
        .domain([d3.min(data), d3.max(data)])
        .range([0, width - padding.left - padding.right]);
      let svg = d3.select("#scale1");

      svg.attr("width", width).attr("height", height);

      let g = svg.append("g").attr("transform", `translate(${padding.left},0)`);

      g.selectAll("circle")
        .data(data)
        .enter()
        .append("circle")
        .attr("r", 3)
        .attr("cx", function (d) {
          return scale(d);
        })
        .attr("cy", function () {
          return (height / 3) * 2;
        });

      g.selectAll("text")
        .data(data)
        .enter()
        .append("text")
        .attr("x", function (d) {
          return scale(d);
        })
        .attr("y", function () {
          return (height / 3) * 1;
        })
        .attr("font-size", 12)
        .text((item) => item)
        .style("text-anchor", "middle");
    },
    handleScale2() {
      let width = 800,
        height = 80;
      let linearScale = d3.scaleLinear().domain([0, 100]).range([0, 700]);

      let sqrtScale = d3.scaleSqrt().domain([0, 100]).range([0, 30]);

      let myData = [0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

      let svg = d3
        .select("#scale2")
        .attr("width", width)
        .attr("height", height);
      svg
        .selectAll("circle")
        .data(myData)
        .enter()
        .append("circle")
        .attr("r", function (d) {
          return sqrtScale(d);
        })
        .attr("cx", function (d) {
          return linearScale(d);
        })
        .attr("cy", function () {
          return "40";
        })
        .attr("fill", "#888");
    },
    handleScale3() {
      let myData = [10, 100, 1000, 10000, 100000];
      let width = 800;
      let height = 80;

      let logScale = d3
        .scaleLog()
        .domain([d3.min(myData), d3.max(myData)])
        .range([0, 700]);

      let svg = d3
        .select("#scale3")
        .attr("width", width)
        .attr("height", height);

      let g = svg.append("g").attr("transform", "translate(40, 40)");
      g.selectAll("text")
        .data(myData)
        .enter()
        .append("text")
        .text((d) => d)
        .attr("r", 5)
        .attr("x", function (d) {
          console.log(logScale(d));
          return logScale(d);
        })
        .attr("y", 40);
    },
    handleScale4() {
      let timeScale = d3
        .scaleTime()
        .domain([new Date(2016, 0, 1), new Date(2017, 6, 1)])
        .range([0, 700]);

      let myData = [
        new Date(2016, 5, 1),
        new Date(2016, 7, 1),
        new Date(2016, 12, 1),
        new Date(2017, 3, 1),
      ];

      let svg = d3.select("#scale4");
      svg.attr("width", 700).attr("height", 80);

      let g = svg.append("g").attr("tranfrom", "translate(40, 40)");
      g.append("line")
        .attr("x1", timeScale(new Date(2016, 0, 1)))
        .attr("y1", 40)
        .attr("x2", timeScale(new Date(2017, 6, 1)))
        .attr("y2", 40)
        .attr("stroke", "black")
        .attr("stroke-width", "1px");
      let points = g
        .selectAll("g")
        .data(myData)
        .enter()
        .append("g")
        .attr("class", "target")
        .attr("transform", function (d) {
          return `translate(${timeScale(d)}, 40)`;
        });
      points
        .append("text")
        .text(
          (d) =>
            `${new Date(d).getFullYear()}/${
              new Date(d).getMonth() + 1
            }/${new Date(d).getDate()}`
        )
        .attr("transform", "translate(0, -15)")
        .style("text-anchor", "middle")
        .style("font-weight", "bolder");
      points.append("circle").attr("r", 5).attr("fill", "gray");
    },
    handleScale5() {
      let linearScale = d3.scaleLinear().domain([0, 100]).range([0, 600]);

      let sequentialScale = d3
        .scaleSequential()
        .domain([0, 100])
        .interpolator(d3.interpolateRainbow);

      let myData = [0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

      let svg = d3.select("#scale5")
      svg.attr("width", 700).attr("height", 80)

      let g = svg.append("g").attr("transform", "translate(40, 0)")
      
      g.selectAll("circle").data(myData).enter().append("circle").attr("r", 5).attr("cx", function(d){
        return linearScale(d)
      }).attr("cy", 40).attr("fill", function(d){ return sequentialScale(d) })

    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  lang="scss">
</style>

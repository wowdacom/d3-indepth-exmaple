<template>
  <div class="container">
    <h1>Chart 1</h1>
    <svg id="shape1" width="700" height="110">
      <path></path>
    </svg>
  </div>
  <div class="container">
    <h1>Chart 2</h1>
    <svg id="shape2"></svg>
  </div>
  <div class="container">
    <h1>Chart 3</h1>
    <svg id="shape3"></svg>
  </div>
  <div class="container">
    <h1>Chart 4</h1>
    <svg id="shape4"></svg>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  name: "Shapes",
  data() {
    return {};
  },
  components: {},
  mounted() {
    this.initChart1();
    this.initChart2();
    this.initChart3();
    this.initChart4();
  },
  methods: {
    initChart1() {
      var lineGenerator = d3.line();
      var points = [
        [0, 80],
        [100, 100],
        [200, 30],
        [300, 50],
        [400, 40],
        [500, 80],
      ];

      var pathData = lineGenerator(points);

      // Select the path element and set its d attribute
      d3.select("path")
        .attr("class", "states")
        .attr("d", pathData)
        .attr("fill", "none")
        .attr("stroke", "#999");
    },
    initChart2() {
      let lineGenerator = d3.line();
      let orangeLine = [
        {
          year: 2017,
          value: 150,
        },
        {
          year: 2018,
          value: 80,
        },
        {
          year: 2019,
          value: 30,
        },
        {
          year: 2020,
          value: 10,
        },
        {
          year: 2021,
          value: 5,
        },
        {
          year: 2022,
          value: 0,
        },
      ];

      let width = 300;
      let height = 450;
      let margin = {
        left: 30,
        right: 20,
        top: 20,
        bottom: 20,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;

      let xScale = d3
        .scalePoint()
        .domain(orangeLine.map((item) => item.year))
        .range([0, innerWidth]);

      let yScale = d3
        .scaleLinear()
        .domain(d3.extent(orangeLine.map((item) => item.value)))
        .range([innerHeight, 0]);

      let svg = d3
        .select("#shape2")
        .attr("width", width)
        .attr("height", height);

      let xAxis = d3.axisBottom(xScale);
      let yAxis = d3.axisLeft(yScale);

      svg
        .append("g")
        .attr("class", "xAxis")
        .call(xAxis)
        .attr(
          "transform",
          `translate(${margin.left}, ${innerHeight + margin.top})`
        );
      svg
        .append("g")
        .attr("class", "yAxis")
        .call(yAxis)
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      let line = lineGenerator
        .x(function (d) {
          return xScale(d.year);
        })
        .y(function (d) {
          return yScale(d.value);
        });

      svg
        .append("g")
        .append("path")
        .attr("d", line(orangeLine))
        .attr("transform", `translate(${margin.left}, ${margin.top})`);
    },
    initChart3() {
      let lineGenerator = d3.line();
      let width = 700;
      let height = 110;
      let margin = {
        left: 30,
        right: 20,
        top: 20,
        bottom: 20,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;
      let points = [[0, 80], [100, 100], null, [300, 50], [400, 40], [500, 80]];

      let svg = d3
        .select("#shape3")
        .attr("width", width)
        .attr("height", height);

      let main = svg
        .append("g")
        .attr("width", innerWidth)
        .attr("height", innerHeight);

      let xScale = d3
        .scaleLinear()
        .domain(
          [-100, 600]
        )
        .range([0, innerWidth]);

      let yScale = d3
        .scaleLinear()
        .domain(
          d3.extent(
            points.filter((item) => item !== null).map((item) => item[1])
          )
        )
        .range([innerHeight, 0]);

      let xAxis = d3.axisBottom(xScale);
      let yAxis = d3.axisLeft(yScale).ticks(3);

      let line = lineGenerator 
        .defined(function (d) {
          return d !== null;
        })
        .x(function (d) {
          return xScale(d[0]);
        })
        .y(function (d) {
          return yScale(d[1]);
        });

      main
        .append("g")
        .attr("class", "xAxis")
        .call(xAxis)
        .attr(
          "transform",
          `translate(${margin.left}, ${margin.top + innerHeight})`
        );
      main
        .append("g")
        .attr("class", "yAxis")
        .call(yAxis)
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      main
        .append("g")
        .append("path")
        .attr("d", line(points))
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      main.selectAll("circle").data(points.filter((item) => item !== null).map((item) => item)).enter().append("circle").attr("r", 5).attr("cx", (d)=>{
          return xScale(d[0]);
      }).attr("cy", function(d){
           return yScale(d[1]);
      }).attr("fill", '#666').attr("transform", `translate(${margin.left}, ${margin.top})`);

      d3.selectAll("g.yAxis g.tick").append("line").attr("x1", 0).attr("y1", 0).attr("x2", innerWidth).attr("y2", 0).attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity", 0.1);

    },
    initChart4() {

    }
  },
};
</script>

<style>
path {
  fill: none;
  stroke: #999;
}
.container {
  margin-bottom: 50px;
}
</style>
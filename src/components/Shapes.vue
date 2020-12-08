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
  <div class="container">
    <h1>Chart 5</h1>
    <svg id="shape5"></svg>
  </div>
  <div class="container">
    <h1>Chart 6</h1>
    <svg id="shape6"></svg>
  </div>
  <div class="container">
    <h1>Chart 7</h1>
    <svg id="shape7"></svg>
  </div>
  <div class="container">
    <h1>Chart 8</h1>
    <svg id="shape8"></svg>
  </div>
  <div class="container">
    <h1>Chart 9</h1>
    <svg id="shape9"></svg>
  </div>
  <div class="container">
    <h1>Chart 10</h1>
    <svg id="shape10"></svg>
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
    this.initChart5();
    this.initChart6();
    this.initChart7();
    this.initChart8();
    this.initChart9();
    this.initChart10();
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

      let xScale = d3.scaleLinear().domain([-100, 600]).range([0, innerWidth]);

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

      main
        .selectAll("circle")
        .data(points.filter((item) => item !== null).map((item) => item))
        .enter()
        .append("circle")
        .attr("r", 5)
        .attr("cx", (d) => {
          return xScale(d[0]);
        })
        .attr("cy", function (d) {
          return yScale(d[1]);
        })
        .attr("fill", "#666")
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      d3.selectAll("g.yAxis g.tick")
        .append("line")
        .attr("x1", 0)
        .attr("y1", 0)
        .attr("x2", innerWidth)
        .attr("y2", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity", 0.1);
    },
    initChart4() {
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
      let points = [
        [0, 80],
        [50, 100],
        [100, 100],
        [300, 50],
        [400, 40],
        [500, 80],
      ];

      let linearScaleX = d3
        .scaleLinear()
        .domain([-50, 550])
        .range([0, innerWidth]);

      let linearScaleY = d3
        .scaleLinear()
        .domain([-50, 150])
        .range([innerHeight, 0]);

      let lineGenerator = d3
        .line()
        .curve(d3.curveCardinal)
        .x(function (d) {
          return linearScaleX(d[0]);
        })
        .y(function (d) {
          return linearScaleY(d[1]);
        });

      let svg = d3
        .select("#shape4")
        .attr("width", width)
        .attr("height", height);
      svg
        .append("g")
        .attr("width", innerWidth)
        .attr("height", innerHeight)
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      let axisX = d3.axisBottom(linearScaleX);
      let axisY = d3.axisLeft(linearScaleY).ticks(3);

      svg
        .append("g")
        .attr("class", "axisX")
        .call(axisX)
        .attr(
          "transform",
          `translate(${margin.left}, ${margin.top + innerHeight})`
        );
      svg
        .append("g")
        .attr("class", "axisY")
        .call(axisY)
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      d3.select("g.axisY")
        .selectAll("g.tick")
        .append("line")
        .attr("x1", 0)
        .attr("y1", 0)
        .attr("x2", innerWidth)
        .attr("y2", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity", 0.1);

      svg
        .append("g")
        .append("path")
        .attr("d", lineGenerator(points))
        .attr("transform", `translate(${margin.left}, ${margin.top})`);
      svg
        .append("g")
        .selectAll("circle")
        .data(points)
        .enter()
        .append("circle")
        .attr("cx", (d) => linearScaleX(d[0]))
        .attr("cy", (d) => linearScaleY(d[1]))
        .attr("r", 5)
        .attr("fill", "gray")
        .attr("transform", `translate(${margin.left}, ${margin.top})`);
      svg
        .append("g")
        .selectAll("circle")
        .data(points)
        .enter()
        .append("circle")
        .attr("cx", (d) => linearScaleX(d[0]))
        .attr("cy", (d) => linearScaleY(d[1]))
        .attr("r", 3)
        .attr("fill", "#fff")
        .attr("transform", `translate(${margin.left}, ${margin.top})`);
    },
    initChart5() {
      let width = 700;
      let height = 200;
      let margin = {
        left: 30,
        right: 20,
        top: 20,
        bottom: 20,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;
      let points = [
        [0, 80],
        [Math.PI * 0.25, 80],
        [Math.PI * 0.5, 30],
        [Math.PI * 0.75, 80],
        [Math.PI, 80],
        [Math.PI * 1.25, 80],
        [Math.PI * 1.5, 80],
        [Math.PI * 1.75, 80],
        [Math.PI * 2, 80],
      ];
      let svg = d3
        .select("#shape5")
        .attr("width", width)
        .attr("height", height);
      let main = svg
        .append("g")
        .attr("width", innerWidth)
        .attr("heigth", innerHeight)
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      let radialLineGenerator = d3.lineRadial();
      let radialLine = radialLineGenerator(points);
      let shapeShift = d3.max(points.map((item) => item[1]));

      main
        .append("path")
        .attr("d", radialLine)
        .attr("transform", `translate(${shapeShift}, ${shapeShift})`);
    },
    initChart6() {
      let width = 700;
      let height = 200;
      let margin = {
        left: 30,
        right: 20,
        top: 20,
        bottom: 20,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;
      let areaGenerator = d3.area().y0(innerHeight);
      let points = [
        [0, 80],
        [100, 100],
        [200, 30],
        [300, 50],
        [400, 40],
        [500, 80],
      ];
      let pathData = areaGenerator(points);
      let svg = d3
        .select("#shape6")
        .attr("width", width)
        .attr("height", height);
      let main = svg
        .append("g")
        .attr("width", innerWidth)
        .attr("height", innerHeight);
      main.append("path").attr("d", pathData);
    },
    initChart7() {
      let width = 700;
      let height = 200;
      let margin = {
        left: 30,
        right: 20,
        top: 20,
        bottom: 20,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;
      let points = [
        { x: 0, low: 30, high: 80 },
        { x: 100, low: 80, high: 100 },
        { x: 200, low: 20, high: 30 },
        { x: 300, low: 20, high: 50 },
        { x: 400, low: 10, high: 40 },
        { x: 500, low: 50, high: 80 },
      ];
      let xScale = d3.scaleLinear().domain([-100, 600]).range([0, innerWidth]);
      let yScale = d3.scaleLinear().domain([0, 110]).range([innerHeight, 0]);
      let xAxis = d3.axisBottom(xScale);
      let yAxis = d3.axisLeft(yScale);

      let areaGenerator = d3
        .area()
        .x(function (d) {
          return xScale(d.x);
        })
        .y0(function (d) {
          return yScale(d.high);
        })
        .y1(function (d) {
          return yScale(d.low);
        });

      let svg = d3
        .select("#shape7")
        .attr("width", width)
        .attr("height", height);
      svg
        .append("g")
        .attr("width", innerWidth)
        .attr("height", innerHeight)
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      svg
        .append("g")
        .attr("class", "axisX")
        .call(xAxis)
        .attr("class", "xAxios")
        .attr(
          "transform",
          `translate(${margin.left}, ${innerHeight + margin.top})`
        );
      svg
        .append("g")
        .attr("class", "axisY")
        .call(yAxis)
        .attr("class", "yAxios")
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      svg
        .append("path")
        .attr("d", areaGenerator(points))
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      d3.selectAll("g.yAxios g.tick")
        .append("line")
        .attr("x0", 0)
        .attr("y0", 0)
        .attr("x1", innerWidth)
        .attr("y1", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity", 0.1);
    },
    initChart8 () {
      let width = 700;
      let height = 200;
      let margin = {
        left: 30,
        right: 20,
        top: 20,
        bottom: 20,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;
      let points = [
        {angle: 0, r0: 30, r1: 90},
        {angle: Math.PI * 0.25, r0: 30, r1: 40},
        {angle: Math.PI * 0.5, r0: 30, r1: 90},
        {angle: Math.PI * 0.75, r0: 30, r1: 40},
        {angle: Math.PI, r0: 30, r1: 90},
        {angle: Math.PI * 1.25, r0: 30, r1: 40},
        {angle: Math.PI * 1.5, r0: 30, r1: 90},
        {angle: Math.PI * 1.75, r0: 30, r1: 40},
        {angle: Math.PI * 2, r0: 30, r1: 90}
      ];
      let xScale = d3.scaleLinear().domain([-100, 600]).range([0, innerWidth]);
      let yScale = d3.scaleLinear().domain([0, 110]).range([innerHeight, 0]);
      let xAxis = d3.axisBottom(xScale);
      let yAxis = d3.axisLeft(yScale);

      let radialAreaGenerator = d3.radialArea()
      .angle(function(d) {
        return d.angle;
      })
      .innerRadius(function(d) {
        return d.r0;
      })
      .outerRadius(function(d) {
        return d.r1;
      });

      let svg = d3
        .select("#shape8")
        .attr("width", width)
        .attr("height", height);

      svg
        .append("g")
        .attr("width", innerWidth)
        .attr("height", innerHeight)
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      svg
        .append("g")
        .call(xAxis)
        .attr("class", "xAxios")
        .attr(
          "transform",
          `translate(${margin.left}, ${innerHeight + margin.top})`
        );

      svg
        .append("g")
        .call(yAxis)
        .attr("class", "yAxios")
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      d3.selectAll("g.yAxios g.tick")
        .append("line")
        .attr("x0", 0)
        .attr("y0", 0)
        .attr("x1", innerWidth)
        .attr("y1", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity", 0.1);

      svg.append("g").append("path").attr("d", radialAreaGenerator(points)).attr("transform", `translate(100, 100)`).attr("fill", "red")
    },
    initChart9 () {

      let width = 700;
      let height = 200;
      let margin = {
        left: 30,
        right: 20,
        top: 20,
        bottom: 20,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;
      let data = [
        {day: 'Mon', apricots: 120, blueberries: 180, cherries: 100},
        {day: 'Tue', apricots: 60,  blueberries: 185, cherries: 105},
        {day: 'Wed', apricots: 100, blueberries: 215, cherries: 110},
        {day: 'Thu', apricots: 80,  blueberries: 230, cherries: 105},
        {day: 'Fri', apricots: 120, blueberries: 240, cherries: 105}
      ];
      let xScale = d3.scaleLinear().domain([0, 600]).range([0, innerWidth]);
      let yScale = d3.scaleBand().domain(data.map(item=>item.day)).range([0, innerHeight])
      let xAxis = d3.axisBottom(xScale);
      let yAxis = d3.axisLeft(yScale);
      var colors = ['#FBB65B', '#513551', '#de3163'];

      let svg = d3
        .select("#shape9")
        .attr("width", width)
        .attr("height", height);

      svg
        .append("g")
        .attr("width", innerWidth)
        .attr("height", innerHeight)
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      svg
        .append("g")
        .call(xAxis)
        .attr("class", "xAxios")
        .attr(
          "transform",
          `translate(${margin.left}, ${innerHeight + margin.top})`
        );

      svg
        .append("g")
        .call(yAxis)
        .attr("class", "yAxios")
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      d3.selectAll("g.yAxios g.tick")
        .append("line")
        .attr("x0", 0)
        .attr("y0", 0)
        .attr("x1", innerWidth)
        .attr("y1", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity", 0.1);

      let myKeys = ['apricots', 'blueberries', 'cherries']
      let stack = d3.stack()
        .keys(myKeys);

      let stackedSeries = stack(data);
      let levelBars = svg.append("g").selectAll("g").data(stackedSeries).enter().append("g")
      .style('fill', function(d, i) {
		return colors[i];
  });
  
      levelBars.selectAll("rect").data(function(d){
        return d
      }).enter().append("rect")
      .attr("width", function(d){
        return xScale(d[1] - d[0])
      })
      .attr("height", 20)
      .attr("x", function(d){
        return xScale(d[0])
      }).attr("y", function(d){
        return yScale(d.data.day)
      })
      .attr("transform", `translate(${margin.left}, ${margin.top + 5})`);

    //  bars.selectAll("rect").data(function(d, i){})


    },
    initChart10 () {

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
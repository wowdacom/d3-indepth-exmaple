<template>
  <div class="container">
    <h1>第一次嘗試</h1>
    <svg id="chart1"></svg>
  </div>
  <div class="container">
    <h1>第二次嘗試</h1>
    <svg id="chart2"></svg>
  </div>
  <div class="container">
    <h1>第三次嘗試</h1>
    <svg id="chart3"></svg>
  </div>
  <div class="container">
    <h1>第四次嘗試</h1>
    <svg id="chart4"></svg>
  </div>
</template>

<script>
import * as d3 from "d3";
import chartdata from "../assets/chart.json";

export default {
  name: "Scales",
  data() {
    return {};
  },
  components: {},
  mounted() {
    this.drawChart1();
    this.drawChart2();
    this.drawChart3();
    this.drawChart4();
  },
  methods: {
    getDigits(myNumber) {
      let counter = 0;
      let i = Math.abs(myNumber);
      while (i >= 10) {
        i /= 10;
        counter += 1;
      }
      if (counter < 3) {
        return 1;
      }
      return Math.floor(i);
    },
    drawChart1() {
      let svg = d3.select("#chart1");
      let xAxisHeight = 280;
      let yAxisWidth = 660;
      let padding = {
        top: 15,
        left: 10,
        bottom: 10,
        right: 10,
      };
      let tickPaddding = 15;
      let priceChg = chartdata.webCaseHistoryPrice.map((item) => item.priceChg);
      let priceChgDate = chartdata.webCaseHistoryPrice.map((item) =>
        new Date(item.priceChgDate).getTime()
      );

      let max = (this.getDigits(d3.max(priceChg)) + 1) * 1000;
      let min = (this.getDigits(d3.min(priceChg)) - 1) * 1000;
      let dateMax = d3.max(priceChgDate);
      let dateMin = d3.min(priceChgDate);
      let xScale = d3.scaleLinear(
        [dateMin, dateMax],
        [
          padding.left + padding.right,
          yAxisWidth - (padding.left + padding.right),
        ]
      );
      let yScale = d3.scaleLinear(
        [max, min],
        [0, xAxisHeight - (padding.top + padding.bottom) - tickPaddding]
      );
      let xAxis = d3
        .axisBottom(xScale)
        .ticks(4)
        .tickFormat(function (d) {
          const months = {
            0: "January",
            1: "February",
            2: "March",
            3: "April",
            4: "May",
            5: "June",
            6: "July",
            7: "August",
            8: "September",
            9: "October",
            10: "November",
            11: "December",
          };
          return `${new Date(
            d
          ).getFullYear()} ${months[new Date(d).getMonth()]}`;
        })
        .tickSize(0)
        .tickPadding(10);
      let yAxis = d3.axisLeft(yScale).ticks(2).tickSize(0).tickPadding(10);
      let xAxisGrid = d3
        .axisBottom(xScale)
        .tickSize(
          -(xAxisHeight - (padding.top + padding.bottom) - tickPaddding)
        )
        .tickFormat("")
        .ticks(10);
      let yAxisGrid = d3
        .axisLeft(yScale)
        .tickSize(-yAxisWidth)
        .tickFormat("")
        .ticks(10);

      svg.attr("width", yAxisWidth).attr("height", xAxisHeight);

      svg
        .append("g")
        .attr("class", "x axis-grid")
        .attr(
          "transform",
          `translate(${padding.left + padding.right + 5},${
            xAxisHeight - padding.top - 15
          })`
        )
        .call(xAxisGrid);

      svg
        .append("g")
        .attr("class", "y axis-grid")
        .attr("transform", `translate(45, ${-5 + padding.top})`)
        .call(yAxisGrid);

      svg
        .append("g")
        .attr(
          "transform",
          `translate(${padding.left + padding.right + 5},${
            xAxisHeight - padding.top - 15
          })`
        )
        .classed("xAxis", true)
        .call(xAxis);

      svg
        .append("g")
        .attr("transform", `translate(45,${-5 + padding.top})`)
        .classed("yAxis", true)
        .call(yAxis);

      //畫菱形上去
      let points = svg
        .append("g")
        .selectAll("g")
        .data(chartdata.webCaseHistoryPrice)
        .enter()
        .append("g")
        .attr("x", function (d) {
          return xScale(new Date(d.priceChgDate));
        })
        .attr("y", function (d) {
          return yScale(d.priceChg);
        })
        .attr("transform", `translate(45,${-5 + padding.top})`);

      points
        .append("rect")
        .attr("fill", "#966bb8")
        .attr("width", 10)
        .attr("height", 10)
        .attr("x", function (d) {
          return xScale(new Date(d.priceChgDate));
        })
        .attr("y", function (d) {
          return yScale(d.priceChg);
        })
        .attr("transform", `translate(45,${-5 + padding.top})`);

      points
        .append("text")
        .text(function (d) {
          const months = {
            0: "January",
            1: "February",
            2: "March",
            3: "April",
            4: "May",
            5: "June",
            6: "July",
            7: "August",
            8: "September",
            9: "October",
            10: "November",
            11: "December",
          };
          return `${new Date(
            d.priceChgDate
          ).getFullYear()} ${months[new Date(d.priceChgDate).getMonth()]}`;
        })
        .attr("class", "foo")
        .attr("x", function (d) {
          return xScale(new Date(d.priceChgDate));
        })
        .attr("y", function (d) {
          return yScale(d.priceChg);
        })
        .style("stroke", "steelblue")
        .style("font-size", "12px")
        .style("font-weight", "bold")
        .attr("transform", `translate(45,${-5 + padding.top})`);

      d3.select("#myCanvas > g.xAxis > path")
        .style("stroke", "#966bb8")
        .style("stroke-width", "2px");
      d3.select("#myCanvas > g.yAxis > path")
        .style("stroke", "#966bb8")
        .style("stroke-width", "2px");
      d3.select("#myCanvas > g.xAxis")
        .selectAll("g")
        .select("text")
        .style("font-size", "13px");
      d3.select("#myCanvas > g.yAxis")
        .selectAll("g")
        .select("text")
        .style("font-size", "13px");
    },
    drawChart2() {
      var data = [
        { letter: "A", frequency: "5.01" },
        { letter: "B", frequency: "7.80" },
        { letter: "C", frequency: "15.35" },
        { letter: "D", frequency: "22.70" },
        { letter: "E", frequency: "34.25" },
        { letter: "F", frequency: "10.21" },
        { letter: "G", frequency: "7.68" },
      ];

      var chartwidth = 500,
        chartheight = 300;

      //removing prior svg elements ie clean up svg
      d3.select("#chart2").selectAll("*").remove();

      //resetting svg height and width in current svg
      d3.select("#chart2").attr("width", chartwidth).attr("height", chartheight);

      //Setting up of our svg with proper calculations
      var svg = d3.select("#chart2");
      var margin = { top: 20, right: 20, bottom: 30, left: 40 };
      var width = svg.attr("width") - margin.left - margin.right;
      var height = svg.attr("height") - margin.top - margin.bottom;

      //Plotting our base area in svg in which chart will be shown
      var g = svg
        .append("g")
        .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

      //X and Y scaling
      var x = d3.scaleBand().rangeRound([0, width]).padding(0.4);
      var y = d3.scaleLinear().rangeRound([height, 0]);

      x.domain(
        data.map(function (d) {
          return d.letter;
        })
      );
      y.domain([
        0,
        d3.max(data, function (d) {
          return +d.frequency;
        }),
      ]);

      //Final Plotting

      //for x axis
      g.append("g")
        .attr("class", "xAxis")
        .call(d3.axisBottom(x))
        .attr("transform", "translate(0," + height + ")");

      //for y axis
      g.append("g")
        .attr("class", "yAxis")
        .call(d3.axisLeft(y))
        .append("text")
        .attr("transform", "rotate(-90)")
        .attr("text-anchor", "end");

      d3.selectAll("g.yAxis g.tick")
        .append("line")
        .attr("class", "gridline")
        .attr("x1", 0)
        .attr("y1", 0)
        .attr("x2", width)
        .attr("y2", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity",.2);

      d3.selectAll("g.xAxis g.tick")
        .append("line")
        .attr("class", "gridline")
        .attr("x1", 0)
        .attr("y1", -height)
        .attr("x2", 0)
        .attr("y2", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity",.2);

      //the line function for path
      var lineFunction = d3
        .line()
        .x(function (d) {
          return x(d.letter);
        })
        .y(function (d) {
          return y(d.frequency);
        })
        .curve(d3.curveLinear);

      //defining the lines
      var path = g.append("path");

      //plotting lines
      path
        .attr("d", lineFunction(data))
        .attr("stroke", "blue")
        .attr("stroke-width", 2)
        .attr("fill", "none");
    },
    drawChart3() {

       var data = [
        { letter: 1, frequency: "5.01" },
        { letter: 2, frequency: "7.80" },
        { letter: 3, frequency: "15.35" },
        { letter: 4, frequency: "22.70" },
        { letter: 5, frequency: "34.25" },
        { letter: 6, frequency: "10.21" },
        { letter: 7, frequency: "7.68" },
      ];

      let width = 660
      let height = 280
      let margin = {
        left: 30,
        top: 20,
        bottom: 25,
        right: 20
      }
      let innerWidth = width - margin.left - margin.right
      let innerHeight = height - margin.top - margin.bottom
      
      let svg = d3.select("#chart3")

      svg.attr("width", width).attr("height", height)

      let letters = data.map(item=>item.letter)
      // let frequemcies = data.map(item=>item.frequency)

      let xScale = d3.scaleLinear().domain([d3.min(letters), d3.max(letters)]).range([0, innerWidth])
      let yScale = d3.scaleLinear().domain([50, 0]).range([0, innerHeight])

      //for x axis
      svg.append("g")
        .attr("class", "xAxis")
        .call(d3.axisBottom(xScale))
        .attr("transform", `translate(${margin.left}, ${innerHeight + margin.top})`);

      //for y axis
      svg.append("g")
        .attr("class", "yAxis")
        .call(d3.axisLeft(yScale))
        .attr("transform", `translate(${margin.left}, ${margin.top})`)
        .append("text")
        .attr("transform", "rotate(-90)")
        .attr("text-anchor", "end");

      let target = svg.append("g").selectAll("g")
         .data(data)
         .enter()
         .append("g")
         .attr("transform", `translate(${margin.left - 5}, ${margin.top - 5})`);


      target
         .append("rect")
         .attr("width", 10)
         .attr("height", 10)
         .attr("fill", "gray")
         .attr("x", function(d){return xScale(d.letter)})
         .attr("y", function(d){
           return yScale(d.frequency)})
         
      target
         .append("text")
         .text(function(d){
           return d.frequency
         })
         .attr("class", "hello")
         .attr("x", function(d){return xScale(d.letter) - 6})
         .attr("y", function(d){
           return yScale(d.frequency) + 10 + 12})
         .style("stroke", "steelblue")
         .style("font-size", "12px")
         .style("font-weight", "bold")

      //加上格線
      d3.selectAll("g.yAxis g.tick")
        .append("line")
        .attr("class", "gridline")
        .attr("x1", 0)
        .attr("y1", 0)
        .attr("x2", innerWidth)
        .attr("y2", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity",.2);

      d3.selectAll("g.xAxis g.tick")
        .append("line")
        .attr("class", "gridline")
        .attr("x1", 0)
        .attr("y1", -innerHeight)
        .attr("x2", 0)
        .attr("y2", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity",.2);

    },
    drawChart4() {

       var data = [
        { letter: 1, frequency: "5.01" },
        { letter: 2, frequency: "7.80" },
        { letter: 3, frequency: "15.35" },
        { letter: 4, frequency: "22.70" },
        { letter: 5, frequency: "34.25" },
        { letter: 6, frequency: "10.21" },
        { letter: 7, frequency: "7.68" },
      ];

      let width = 660
      let height = 280
      let margin = {
        left: 30,
        top: 20,
        bottom: 25,
        right: 20
      }
      let innerWidth = width - margin.left - margin.right
      let innerHeight = height - margin.top - margin.bottom
      
      let svg = d3.select("#chart4")

      svg.attr("width", width).attr("height", height)

      let letters = data.map(item=>item.letter)
      // let frequemcies = data.map(item=>item.frequency)

      let xScale = d3.scaleLinear().domain([d3.min(letters), d3.max(letters)]).range([0, innerWidth])
      let yScale = d3.scaleLinear().domain([50, 0]).range([0, innerHeight])

      //for x axis
      svg.append("g")
        .attr("class", "xAxis")
        .call(d3.axisBottom(xScale))
        .attr("transform", `translate(${margin.left}, ${innerHeight + margin.top})`);

      //for y axis
      svg.append("g")
        .attr("class", "yAxis")
        .call(d3.axisLeft(yScale))
        .attr("transform", `translate(${margin.left}, ${margin.top})`)
        .append("text")
        .attr("transform", "rotate(-90)")
        .attr("text-anchor", "end");

      let target = svg.append("g").selectAll("g")
         .data(data)
         .enter()
         .append("g")
         .attr("transform", `translate(${margin.left - 5}, ${margin.top - 5})`);


      target
         .append("rect")
         .attr("width", 10)
         .attr("height", 10)
         .attr("fill", "gray")
         .attr("x", function(d){return xScale(d.letter)})
         .attr("y", function(d){
           return yScale(d.frequency)})
         
      target
         .append("text")
         .text(function(d){
           return d.frequency
         })
         .attr("class", "hello")
         .attr("x", function(d){return xScale(d.letter) - 6})
         .attr("y", function(d){
           return yScale(d.frequency) + 10 + 12})
         .style("stroke", "steelblue")
         .style("font-size", "12px")
         .style("font-weight", "bold")

      //加上格線
      d3.selectAll("g.yAxis g.tick")
        .append("line")
        .attr("class", "gridline")
        .attr("x1", 0)
        .attr("y1", 0)
        .attr("x2", innerWidth)
        .attr("y2", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity",.2);

      d3.selectAll("g.xAxis g.tick")
        .append("line")
        .attr("class", "gridline")
        .attr("x1", 0)
        .attr("y1", -innerHeight)
        .attr("x2", 0)
        .attr("y2", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity",.2);

      var line = d3.line()
      .x(function(d) { return xScale(d.letter); }) // set the x values for the line generator
      .y(function(d) { return yScale(d.frequency); }) // set the y values for the line generator 
      .curve(d3.curveMonotoneX)

      //plotting lines

      svg.append("path")
        .datum(data) // 10. Binds data to the line 
        .attr("class", "line") // Assign a class for styling 
        .attr("d", line)
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  lang="scss">
#myCanvas {
  g.yAxis,
  g.xAxis {
    path {
      stroke: #966bb8;
      stroke-width: 2px;
    }
    text {
      font-family: sans-serif;
      font-size: 12px;
      font-weight: bolder;
    }
  }
  g {
    g {
      rect {
        // transform-origin: center center;
        // transform: rotate(45deg);
      }
    }
  }
  .axis-grid line {
    stroke: #def;
  }
  .gridline{
    stroke: black;
    shape-rendering: crispEdges;
    stroke-opacity: .2;
  }
}
.container {
  display: flex;
  align-items: center;
  flex-direction: column;
}
.line {
    fill: none;
    stroke: rgba(157, 104, 199, 0.5);
    stroke-width: 3;
}
</style>

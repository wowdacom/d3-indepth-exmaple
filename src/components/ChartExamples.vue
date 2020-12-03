<template>
  <div class="container">
    <h1>Chart 1</h1>
    <svg id="chart1"></svg>
  </div>
  <div class="container">
    <h1>Chart 2</h1>
    <svg id="chart2"></svg>
  </div>
  <div class="container">
    <h1>Chart 3</h1>
    <svg id="chart3"></svg>
  </div>
  <div class="container">
    <h1>Chart 4</h1>
    <svg id="chart4"></svg>
  </div>
  <div class="container">
    <h1>Chart 5</h1>
    <div id="chart5"></div>
  </div>
  <div class="container">
    <h1>Chart 6</h1>
    <svg id="chart6"></svg>
  </div>
  <div class="container">
    <h1>Chart 7</h1>
    <div id="chart7"></div>
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
    this.drawChart5();
    this.drawChart6();
    this.drawChart7();
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
      let data = [
        { letter: "A", frequency: "5.01" },
        { letter: "B", frequency: "7.80" },
        { letter: "C", frequency: "15.35" },
        { letter: "D", frequency: "22.70" },
        { letter: "E", frequency: "34.25" },
        { letter: "F", frequency: "10.21" },
        { letter: "G", frequency: "7.68" },
      ];

      let chartwidth = 500,
        chartheight = 300;

      //removing prior svg elements ie clean up svg
      d3.select("#chart2").selectAll("*").remove();

      //resetting svg height and width in current svg
      d3.select("#chart2")
        .attr("width", chartwidth)
        .attr("height", chartheight);

      //Setting up of our svg with proper calculations
      let svg = d3.select("#chart2");
      let margin = { top: 20, right: 20, bottom: 30, left: 40 };
      let width = svg.attr("width") - margin.left - margin.right;
      let height = svg.attr("height") - margin.top - margin.bottom;

      //Plotting our base area in svg in which chart will be shown
      let g = svg
        .append("g")
        .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

      //X and Y scaling
      let x = d3.scaleBand().rangeRound([0, width]).padding(0.4);
      let y = d3.scaleLinear().rangeRound([height, 0]);

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
        .attr("stroke-opacity", 0.2);

      d3.selectAll("g.xAxis g.tick")
        .append("line")
        .attr("class", "gridline")
        .attr("x1", 0)
        .attr("y1", -height)
        .attr("x2", 0)
        .attr("y2", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity", 0.2);

      //the line function for path
      let lineFunction = d3
        .line()
        .x(function (d) {
          return x(d.letter);
        })
        .y(function (d) {
          return y(d.frequency);
        })
        .curve(d3.curveLinear);

      //defining the lines
      let path = g.append("path");

      //plotting lines
      path
        .attr("d", lineFunction(data))
        .attr("stroke", "blue")
        .attr("stroke-width", 2)
        .attr("fill", "none");
    },
    drawChart3() {
      let data = [
        { letter: 1, frequency: "5.01" },
        { letter: 2, frequency: "7.80" },
        { letter: 3, frequency: "15.35" },
        { letter: 4, frequency: "22.70" },
        { letter: 5, frequency: "34.25" },
        { letter: 6, frequency: "10.21" },
        { letter: 7, frequency: "7.68" },
      ];

      let width = 660;
      let height = 280;
      let margin = {
        left: 30,
        top: 20,
        bottom: 25,
        right: 20,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;

      let svg = d3.select("#chart3");

      svg.attr("width", width).attr("height", height);

      let letters = data.map((item) => item.letter);
      // let frequemcies = data.map(item=>item.frequency)

      let xScale = d3
        .scaleLinear()
        .domain([d3.min(letters), d3.max(letters)])
        .range([0, innerWidth]);
        
      let yScale = d3.scaleLinear().domain([50, 0]).range([0, innerHeight]);

      //for x axis
      svg
        .append("g")
        .attr("class", "xAxis")
        .call(d3.axisBottom(xScale))
        .attr(
          "transform",
          `translate(${margin.left}, ${innerHeight + margin.top})`
        );

      //for y axis
      svg
        .append("g")
        .attr("class", "yAxis")
        .call(d3.axisLeft(yScale))
        .attr("transform", `translate(${margin.left}, ${margin.top})`)
        .append("text")
        .attr("transform", "rotate(-90)")
        .attr("text-anchor", "end");

      let target = svg
        .append("g")
        .selectAll("g")
        .data(data)
        .enter()
        .append("g")
        .attr("transform", `translate(${margin.left - 5}, ${margin.top - 5})`);

      target
        .append("rect")
        .attr("width", 10)
        .attr("height", 10)
        .attr("fill", "gray")
        .attr("x", function (d) {
          return xScale(d.letter);
        })
        .attr("y", function (d) {
          return yScale(d.frequency);
        });

      target
        .append("text")
        .text(function (d) {
          return d.frequency;
        })
        .attr("class", "hello")
        .attr("x", function (d) {
          return xScale(d.letter) - 6;
        })
        .attr("y", function (d) {
          return yScale(d.frequency) + 10 + 12;
        })
        .style("stroke", "steelblue")
        .style("font-size", "12px")
        .style("font-weight", "bold");

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
        .attr("stroke-opacity", 0.2);

      d3.selectAll("g.xAxis g.tick")
        .append("line")
        .attr("class", "gridline")
        .attr("x1", 0)
        .attr("y1", -innerHeight)
        .attr("x2", 0)
        .attr("y2", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity", 0.2);
    },
    drawChart4() {
      let data = [
        { letter: 1, frequency: "5.01" },
        { letter: 2, frequency: "7.80" },
        { letter: 3, frequency: "15.35" },
        { letter: 4, frequency: "22.70" },
        { letter: 5, frequency: "34.25" },
        { letter: 6, frequency: "10.21" },
        { letter: 7, frequency: "7.68" },
        { letter: 8, frequency: "16.57" },
        { letter: 9, frequency: "13.66" },
      ];

      let width = 660;
      let height = 280;
      let margin = {
        left: 30,
        top: 20,
        bottom: 25,
        right: 20,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;

      let svg = d3.select("#chart4");

      svg.attr("width", width).attr("height", height);

      let letters = data.map((item) => item.letter);
      // let frequemcies = data.map(item=>item.frequency)

      let xScale = d3
        .scaleLinear()
        .domain([d3.min(letters), d3.max(letters)])
        .range([0, innerWidth]);
      let yScale = d3.scaleLinear().domain([50, 0]).range([0, innerHeight]);

      //for x axis
      svg
        .append("g")
        .attr("class", "xAxis")
        .call(d3.axisBottom(xScale))
        .attr(
          "transform",
          `translate(${margin.left}, ${innerHeight + margin.top})`
        );

      //for y axis
      svg
        .append("g")
        .attr("class", "yAxis")
        .call(d3.axisLeft(yScale))
        .attr("transform", `translate(${margin.left}, ${margin.top})`)
        .append("text")
        .attr("transform", "rotate(-90)")
        .attr("text-anchor", "end");

      let target = svg
        .append("g")
        .selectAll("g")
        .data(data)
        .enter()
        .append("g")
        .attr("transform", `translate(${margin.left - 5}, ${margin.top - 5})`);
      target
        .append("rect")
        .attr("width", 10)
        .attr("height", 10)
        .attr("fill", "gray")
        .attr("x", function (d) {
          return xScale(d.letter);
        })
        .attr("y", function (d) {
          return yScale(d.frequency);
        })
        .attr("transform", function (d) {
          return `rotate(45,${
            xScale(d.letter) + 10 / 2
          },${yScale(d.frequency) + 10 / 2})`;
        });

      //https://stackoverflow.com/questions/10985077/d3-js-odd-rotation-behavior

      target
        .append("text")
        .text(function (d) {
          return d.frequency;
        })
        .attr("class", "hello")
        .attr("x", function (d) {
          return xScale(d.letter) - 6;
        })
        .attr("y", function (d) {
          return yScale(d.frequency) + 10 + 12;
        })
        .style("stroke", "steelblue")
        .style("font-size", "12px")
        .style("font-weight", "bold");

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
        .attr("stroke-opacity", 0.2);

      d3.selectAll("g.xAxis g.tick")
        .append("line")
        .attr("class", "gridline")
        .attr("x1", 0)
        .attr("y1", -innerHeight)
        .attr("x2", 0)
        .attr("y2", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity", 0.2);

      let line = d3
        .line()
        .x(function (d) {
          return xScale(d.letter);
        }) // set the x values for the line generator
        .y(function (d) {
          return yScale(d.frequency);
        }) // set the y values for the line generator
        .curve(d3.curveMonotoneX);

      //plotting lines

      svg
        .append("path")
        .datum(data) // 10. Binds data to the line
        .attr("class", "line") // Assign a class for styling
        .attr("d", line)
        .attr("transform", `translate(${margin.left}, ${margin.top})`);
    },
    drawChart5() {
      let width = 660,
        height = 280;

      let data = [
        { letter: 1, frequency: "5.01" },
        { letter: 2, frequency: "7.80" },
        { letter: 3, frequency: "15.35" },
        { letter: 4, frequency: "22.70" },
        { letter: 5, frequency: "34.25" },
        { letter: 6, frequency: "10.21" },
        { letter: 7, frequency: "7.68" },
        { letter: 8, frequency: "16.57" },
        { letter: 9, frequency: "13.66" },
      ];

      let data_table = new Array(3);

      for (let i = 0; i < data_table.length; i++) {
        data_table[i] = new Array(3);
      }

      data.map(function (item, index) {
        data_table[Math.floor(index / 3)][index % 3] = item;
      });

      let table = d3.select("#chart5").append("table");

      table.attr("width", width).attr("height", height);

      let thead = table.append("thead");
      let tbody = table.append("tbody");

      let colorScale = d3
        .scaleLinear()
        .domain([0, 50])
        .range(["rgba(34,193,195, 0.5)", "rgba(253,187,45, 0.5)"]);

      thead
        .append("tr")
        .append("th")
        .attr("colspan", 3)
        .text("熱圖")
        .style("text-align", "center")
        .style("background-color", "gray")
        .style("color", "#fff");

      let rows = tbody.selectAll("tr").data(data_table).enter().append("tr");

      rows
        .selectAll("td")
        .data(function (d) {
          return d;
        })
        .enter()
        .append("td")
        .text(function (d) {
          return d.frequency;
        })
        .style("font-size", "2rem")
        .style("text-align", "center")
        .style("color", "#fff")
        .style("background-color", function (d) {
          return colorScale(d.frequency);
        });
    },
    drawChart6() {
      let data = [1.1, 2.2, 4.46, 2.12, 1.36, 5.002445, 4.1242];

      // Selecting SVG using d3.select()
      let svg = d3.select("#chart6");
      svg.attr("width", 300).attr("height", 300);

      let g = svg.append("g").attr("transform", "translate(150,150)");

      // Creating Pie generator
      let pie = d3.pie();

      // Creating arc
      let arc = d3.arc().innerRadius(0).outerRadius(100);

      // Grouping different arcs
      let arcs = g
        .selectAll("arc")
        .data(function () {
          return pie(data);
        })
        .enter()
        .append("g");

      // Appending path
      arcs
        .append("path")
        .attr("fill", (data, i) => {
          return d3.schemeSet3[i];
        })
        .attr("d", arc);
    },
    drawChart7() {
      let width = 400,
        height = 400;

      let data = [
        { letter: 1, frequency: "5.01" },
        { letter: 2, frequency: "7.80" },
        { letter: 3, frequency: "15.35" },
        { letter: 4, frequency: "22.70" },
        { letter: 5, frequency: "34.25" },
        { letter: 6, frequency: "10.21" },
        { letter: 7, frequency: "7.68" },
        { letter: 8, frequency: "16.57" },
        { letter: 9, frequency: "13.66" },
      ];

      let svg = d3.select("#chart7").append("svg");

      svg.attr("width", width).attr("height", height);

      let g = svg
        .append("g")
        .attr("transform", `translate(${width / 2}, ${height / 2})`);

      let pie = d3.pie();

      let arc = d3.arc().innerRadius(0).outerRadius(100);

      let arcs = g
        .selectAll("arc")
        .data(function () {
          return pie(data.map((item) => item.frequency));
        })
        .enter()
        .append("g")
        .on("mouseover", handleMouseOver)
        .on("mouseout", handleMouseOut);

      arcs
        .append("path")
        .attr("fill", (data, i) => {
          // let value=data.data;
          return d3.schemeSet3[i];
        })
        .attr("d", arc)
        
      arcs
        .append("text")
        .attr("transform", function (data) {
          var _data = arc.centroid(data);
          _data[0] = _data[0] * 3;
          _data[1] = _data[1] * 3;

          return "translate(" + _data + ")";
        })
        .style("text-anchor", "middle")
        .text(function (d) {
          return d.data;
        })
        .style("opacity", 0);

      arcs
        .append("line")
        .attr("x1", function (d) {
          return arc.centroid(d)[0] * 1.5;
        })
        .attr("y1", function (d) {
          return arc.centroid(d)[1] * 1.5;
        })
        .attr("x2", function (d) {
          return arc.centroid(d)[0] * 2.5;
        })
        .attr("y2", function (d) {
          return arc.centroid(d)[1] * 2.5;
        })
        .attr("stroke", "black")
        .attr("stroke-width", "1px")
        .style("opacity", 0);

      // Create Event Handlers for mouse
      function handleMouseOver() {
        // Add interactivity
        d3.select(this).select("path").transition().duration(500).attr("transform", "scale(1.1)").style("opacity", 0.5);
        d3.select(this).select("text").transition().duration(500).style("opacity", 1);
        d3.select(this).select("line").transition().duration(500).style("opacity", 1);
      }

      function handleMouseOut() {
        d3.select(this).select("path").transition().duration(100).attr("transform", "scale(1)").style("opacity", 1);
        d3.select(this).select("text").transition().duration(100).style("opacity", 0);
        d3.select(this).select("line").transition().duration(100).style("opacity", 0);
      }
    },
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
  .gridline {
    stroke: black;
    shape-rendering: crispEdges;
    stroke-opacity: 0.2;
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

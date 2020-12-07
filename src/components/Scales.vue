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
  <div class="container">
    <h1>Chart 6</h1>
    <svg id="scale6"></svg>
    <button @click="handleScale6">Change Scale</button>
  </div>
  <div class="container">
    <h1>Chart 7</h1>
    <svg id="scale7"></svg>
  </div>
  <div class="container">
    <h1>Chart 8</h1>
    <svg id="scale8"></svg>
    <div class="info">Click on the grey band</div>
  </div>
  <div class="container">
    <h1>Chart 9</h1>
    <svg id="scale9"></svg>
  </div>
  <div class="container">
    <h1>Chart 10</h1>
    <svg id="scale10"></svg>
  </div>
  <div class="container">
    <h1>Chart 11</h1>
    <svg id="scale11"></svg>
  </div>
  <div class="container">
    <h1>Chart 12</h1>
    <svg id="scale12"></svg>
  </div>
  <div class="container">
    <h1>Chart 13</h1>
    <svg id="scale13"></svg>
  </div>
  <div class="container">
    <h1>Chart 14</h1>
    <svg id="scale14"></svg>
  </div>
</template>

<script>
import * as d3 from "d3";
// import chartdata from "../assets/chart.json";

export default {
  name: "Scales",
  data() {
    return {
      isMeasure: true,
    };
  },
  components: {},
  mounted() {
    this.handleScale1();
    this.handleScale2();
    this.handleScale3();
    this.handleScale4();
    this.handleScale5();
    this.handleScale6();
    this.handleScale7();
    this.handleScale8();
    this.handleScale9();
    this.handleScale10();
    this.handleScale11();
    this.handleScale12();
    this.handleScale13();
    this.handleScale14();
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

      let svg = d3.select("#scale5");
      svg.attr("width", 700).attr("height", 80);

      let g = svg.append("g").attr("transform", "translate(40, 0)");

      g.selectAll("circle")
        .data(myData)
        .enter()
        .append("circle")
        .attr("r", 5)
        .attr("cx", function (d) {
          return linearScale(d);
        })
        .attr("cy", 40)
        .attr("fill", function (d) {
          return sequentialScale(d);
        });
    },
    handleScale6() {
      let data = [0.243, 0.584, 0.987, 0.153, 0.433];
      let extent = d3.extent(data);

      let width = 700;
      let height = 80;
      let margin = {
        left: 10,
        right: 10,
        top: 10,
        bottom: 10,
      };
      let innerWidth = width - margin.left - margin.right;

      let linearScale = d3.scaleLinear().domain(extent).range([0, innerWidth]);

      d3.selectAll(".tick").remove();
      if (this.isMeasure) {
        linearScale.nice();
      }

      let svg = d3.select("#scale6");
      svg.attr("width", width).attr("height", height);

      let axis = d3.axisBottom(linearScale);

      let g = svg
        .append("g")
        .attr("class", "tick")
        .attr("transform", `translate(${margin.left},0)`);

      g.attr("class", "axis").call(axis);

      this.isMeasure = !this.isMeasure;
    },
    handleScale7() {
      let linearScale = d3.scaleLinear().domain([-100, 100]).range([0, 600]);
      let linearScaleColor = d3
        .scaleLinear()
        .domain([-100, 0, 100])
        .range(["red", "#ddd", "blue"]);

      let myData = [-100, -80, -60, -40, -20, 0, 20, 40, 60, 80, 100];

      let svg = d3.select("#scale7");
      svg.attr("width", 700).attr("height", 80);

      let g = svg.append("g").attr("transform", "translate(40, 0)");

      g.selectAll("circle")
        .data(myData)
        .enter()
        .append("circle")
        .attr("r", 5)
        .attr("cx", function (d) {
          return linearScale(d);
        })
        .attr("cy", 40)
        .attr("fill", function (d) {
          return linearScaleColor(d);
        });
    },
    handleScale8() {
      let svg = d3.select("#scale8");
      let width = 700;
      let height = 80;
      let margin = {
        left: 10,
        right: 10,
        top: 10,
        bottom: 10,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;
      let linearScale = d3
        .scaleLinear()
        .domain([-50, 50])
        .range([0, innerWidth]);
      let touchBoxHeight = (80 / 4) * 3;

      let axis = d3.axisBottom(linearScale);

      svg.attr("width", width).attr("height", height);

      svg
        .append("rect")
        .attr("class", "touch-area")
        .attr("width", innerWidth)
        .attr("height", touchBoxHeight)
        .attr("fill", "gray")
        .attr("transform", `translate(${margin.left}, 0)`)
        .on("click", function (event) {
          let pos = d3.pointer(event, this);
          let xPos = pos[0];
          let value = linearScale.invert(xPos);
          d3.select(".info").text("You click " + value.toFixed(2));
        });

      let g = svg
        .append("g")
        .attr("class", "tick")
        .attr("transform", `translate(${margin.left},${innerHeight})`);

      g.attr("class", "axisX").call(axis);
    },
    handleScale9() {
      let myData = d3.range(0, 100);
      let width = 700;
      let height = 80;
      let margin = {
        left: 10,
        right: 10,
        top: 10,
        bottom: 10,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;
      let quantizeScale = d3
        .scaleQuantize()
        .domain([0, 100])
        .range(["lightblue", "orange", "lightgreen", "pink"]);

      let linearScale = d3
        .scaleLinear()
        .domain([0, 100])
        .range([0, innerWidth]);

      let svg = d3
        .select("#scale9")
        .attr("width", width)
        .attr("height", height);
      let g = svg.append("g").attr("transform", `translate(${margin.left}, 0)`);
      g.selectAll("rect")
        .data(myData)
        .enter()
        .append("rect")
        .attr("x", function (d) {
          return linearScale(d);
        })
        .attr("y", margin.top)
        .attr("width", (innerWidth / myData.length) * 0.5)
        .attr("height", innerHeight)
        .attr("fill", function (d) {
          return quantizeScale(d);
        });
    },
    handleScale10() {
      let myData = [0, 5, 7, 10, 20, 30, 35, 40, 60, 62, 65, 70, 80, 90, 100];
      let extent = d3.extent(myData);
      let quantileScale = d3
        .scaleQuantize()
        .domain(extent)
        .range(["lightblue", "orange", "lightgreen", "pink"]);

      let width = 700;
      let height = 80;
      let margin = {
        left: 10,
        right: 10,
        top: 10,
        bottom: 10,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;
      let linearScale = d3.scaleLinear().domain(extent).range([0, innerWidth]);

      let svg = d3
        .select("#scale10")
        .attr("width", width)
        .attr("height", height);
      let g = svg
        .append("g")
        .attr(
          "transform",
          `translate(${margin.left}, ${innerHeight / 2 - 2.5})`
        );
      g.selectAll("circle")
        .data(myData)
        .enter()
        .append("circle")
        .attr("r", 5)
        .attr("cx", (d) => {
          return linearScale(d);
        })
        .attr("cy", margin.top)
        .attr("fill", function (d) {
          return quantileScale(d);
        });
    },
    handleScale11() {
      let width = 700;
      let height = 80;
      let margin = {
        left: 10,
        right: 10,
        top: 10,
        bottom: 10,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;
      let cells = d3.range(-100, 200, 2);
      let extent = d3.extent(cells);
      let linearScale = d3.scaleLinear().domain(extent).range([0, innerWidth]);

      let thresholdScale = d3
        .scaleThreshold()
        .domain([-50, 50, 150])
        .range(["#ccc", "lightblue", "orange", "#ccc"]);

      let svg = d3
        .select("#scale11")
        .attr("width", width)
        .attr("height", height);

      let g = svg.append("g").attr("transform", `translate(${margin.left}, 0)`);
      g.selectAll("rect")
        .data(cells)
        .enter()
        .append("rect")
        .attr("width", 3)
        .attr("height", innerHeight)
        .attr("x", function (d) {
          return linearScale(d);
        })
        .attr("y", margin.top)
        .attr("fill", function (d) {
          return thresholdScale(d);
        });
    },
    handleScale12() {
      let myData = [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Sep",
        "Oct",
        "Nov",
        "Dec",
      ];
      let width = 700;
      let height = 80;
      let margin = {
        left: 10,
        right: 10,
        top: 10,
        bottom: 10,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;
      let linearScale = d3
        .scaleLinear()
        .domain([0, myData.length])
        .range([0, innerWidth]);

      let ordinalScale = d3
        .scaleOrdinal()
        .domain(myData)
        .range(["black", "#ccc", "#ccc"]);

      let svg = d3
        .select("#scale12")
        .attr("width", width)
        .attr("height", height);
      let g = svg
        .append("g")
        .attr("width", innerWidth)
        .attr("height", innerHeight)
        .attr("transform", `translate(${margin.left}, ${innerHeight / 2})`);

      g.selectAll("text")
        .data(myData)
        .enter()
        .append("text")
        .text(function (d) {
          return d;
        })
        .attr("x", function (d, i) {
          return linearScale(i);
        })
        .attr("y", margin.top)
        .attr("fill", function (d) {
          return ordinalScale(d);
        });
    },
    handleScale13() {
      let myData = [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Sep",
        "Oct",
        "Nov",
        "Dec",
      ];
      let width = 700;
      let height = 80;
      let margin = {
        left: 10,
        right: 10,
        top: 10,
        bottom: 10,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;
      let linearScale = d3
        .scaleLinear()
        .domain([0, myData.length])
        .range([0, innerWidth]);

      let ordinalScale = d3
        .scaleOrdinal()
        .domain(myData)
        .range(d3.schemePaired);

      let svg = d3
        .select("#scale13")
        .attr("width", width)
        .attr("height", height);
      let g = svg
        .append("g")
        .attr("width", innerWidth)
        .attr("height", innerHeight)
        .attr("transform", `translate(${margin.left}, ${innerHeight / 2})`);

      g.selectAll("text")
        .data(myData)
        .enter()
        .append("text")
        .text(function (d) {
          return d;
        })
        .attr("x", function (d, i) {
          return linearScale(i);
        })
        .attr("y", margin.top)
        .attr("fill", function (d) {
          return ordinalScale(d);
        });
    },
    handleScale14() {
      let myData = [
        {
          score: 30,
          name: 'Jim'
        },
        {
          score: 60,
          name: 'John'
        },
        {
          score: 75,
          name: 'Alex'
        },
        {
          score: 25,
          name: 'Peter'
        },
        {
          score: 95,
          name: 'Howard'
        }
      ]
      let width = 300;
      let height = 450;
      let margin = {
        left: 25,
        right: 10,
        top: 22,
        bottom: 20,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;

      //畫出基本的圖形
      let svg = d3.select("#scale14")
      let bandScale = d3.scaleBand()
                        .domain(myData.map(item=>item.name))
                        .range([0, innerWidth])
      
      let linearScale = d3.scaleLinear()
                          .domain([0, 100])
                          .range([0, innerHeight])
                          .nice()

      let scaleAxis = d3.scaleLinear()
                          .domain([0, 100])
                          .range([innerHeight, 0])
                          .nice()

      svg.attr("width", width).attr("height", height);

      let g = svg.append("g").attr("width", innerWidth).attr("height", innerHeight).attr("transform", `translate(${margin.left}, ${margin.top})`);

      g.selectAll("rect").data(myData).enter().append("rect").attr("width", (innerWidth/myData.length) - 5).attr("height", function(d){
        return linearScale(d.score)
      }).attr("fill", "orange").attr("x", function(d){
        return bandScale(d.name)
      }).attr("y", function(d){
        return innerHeight - linearScale(d.score)
      })


      //加上 axisX axisY
      let axisX = svg.append("g").attr("class", "yAxis").selectAll("text").data(myData).enter().append("text").text(function(d){
        return d.name
      })
      let axisY = d3.axisLeft(scaleAxis);
      axisX.attr("x", function(d){
        return bandScale(d.name) + ((innerWidth/myData.length) - 5)/2
      }).attr("y", innerHeight + margin.bottom)
      .attr("transform", `translate(${margin.left}, ${margin.top})`)
      .style("text-anchor", "middle");

      let gY = svg
        .append("g")
        .attr("class", "tick")
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      gY.attr("class", "axisY").call(axisY);

      //加上基準線
      d3.selectAll("g.tick")
        .append("line")
        .attr("class", "gridline")
        .attr("x1", 0)
        .attr("y1", 0)
        .attr("x2", innerWidth)
        .attr("y2", 0)
        .attr("stroke", "black")
        .attr("shape-rendering", "crispEdges")
        .attr("stroke-opacity", 0.1);
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>

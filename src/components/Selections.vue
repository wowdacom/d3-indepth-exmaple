<template>
  <div id="Selections">
    <div class="containter">
      <h1>Chart 1</h1>
      <svg id="chart-1" width="760" height="140">
        <g transform="translate(70, 70)">
          <circle r="40" fill="#ddd"></circle>
          <circle r="40" cx="120" fill="#ddd"></circle>
          <circle r="40" cx="240" fill="#ddd"></circle>
          <circle r="40" cx="360" fill="#ddd"></circle>
          <circle r="40" cx="480" fill="#ddd"></circle>
        </g>
      </svg>
    </div>
    <div class="containter">
      <h1>Chart 2</h1>
      <svg id="chart-2" width="760" height="190">
        <g transform="translate(70, 20)">
          <rect width="30" height="30" y="0" x="0"></rect>
          <rect width="30" height="30" y="40" x="0"></rect>
          <rect width="30" height="30" y="80" x="0"></rect>
          <rect width="30" height="30" y="120" x="0"></rect>
        </g>
      </svg>
      <button @click="handleChangeChart2">Change Position</button>
    </div>
    <div class="container">
      <h1>Chart 3</h1>
      <svg id="chart-3" width="760" height="140">
        <g transform="translate(70, 70)">
          <circle r="40"></circle>
          <circle r="40" cx="120"></circle>
          <circle r="40" cx="240"></circle>
          <circle r="40" cx="360"></circle>
          <circle r="40" cx="480"></circle>
        </g>
      </svg>
      <div class="status">You clicked on circle 1</div>
      <table class="table table-bordered">
        <tbody>
          <tr>
            <th>Event name</th>
            <th>Description</th>
          </tr>
          <tr>
            <td>click</td>
            <td>Element has been clicked</td>
          </tr>
          <tr>
            <td>mouseenter</td>
            <td>Mouse pointer has moved onto the element</td>
          </tr>
          <tr>
            <td>mouseover</td>
            <td>Mouse pointer has moved onto the element or its children</td>
          </tr>
          <tr>
            <td>mouseleave</td>
            <td>Mouse pointer has moved off the element</td>
          </tr>
          <tr>
            <td>mouseout</td>
            <td>Mouse pointer has moved off the element or its children</td>
          </tr>
          <tr>
            <td>mousemove</td>
            <td>Mouse pointer has moved over the element</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="container">
      <h1>Chart 4</h1>
      <svg id="chart-4" width="760" height="140">
        <g transform="translate(70, 70)">
          <circle r="40"></circle>
          <circle r="40" cx="120"></circle>
          <circle r="40" cx="240"></circle>
          <circle r="40" cx="360"></circle>
          <circle r="40" cx="480"></circle>
        </g>
      </svg>
    </div>
    <div class="container">
      <h1>Chart 5</h1>
      <svg id="chart-5" width="760" height="140">
        <g class="item" transform="translate(70, 50)">
          <circle r="40" />
        </g>
        <g class="item" transform="translate(190, 50)">
          <circle r="40" />
        </g>
        <g class="item" transform="translate(310, 50)">
          <circle r="40" />
        </g>
      </svg>
    </div>
    <div class="container">
      <h1>Chart 6</h1>
      <svg id="chart-6" width="760" height="140">
        <g class="item" transform="translate(70, 50)">
          <circle r="40" />
        </g>
        <g class="item" transform="translate(190, 50)">
          <circle r="40" />
        </g>
        <g class="item" transform="translate(310, 50)">
          <circle r="40" />
        </g>
      </svg>
      <button @click="handleChangeChart6">Remove Circle</button>
    </div>
    <div class="container">
      <h1>Chart 7</h1>
      <div class="chart-7">
        <svg id="chart-7" width="760" height="140">
          <g transform="translate(70, 70)">
            <circle r="20" style="fill: orange"></circle>
            <circle r="20" cx="120" style="fill: orange"></circle>
            <circle r="20" cx="240" style="fill: orange"></circle>
            <circle r="20" cx="360" style="fill: orange"></circle>
            <circle r="20" cx="480" style="fill: orange"></circle>
          </g>
        </svg>
        <div class="status">You clicked on circle 1</div>
      </div>
    </div>
    <div class="container">
      <h1>Chart 8</h1>
      <svg id="chart-8" width="760" height="140">
        <g transform="translate(70, 70)">
          <circle r="20" style="fill: orange"></circle>
          <circle r="20" cx="120" style="fill: orange"></circle>
          <circle r="20" cx="240" style="fill: orange"></circle>
          <circle r="20" cx="360" style="fill: orange"></circle>
          <circle r="20" cx="480" style="fill: orange"></circle>
        </g>
      </svg>
    </div>
    <div class="container">
      <h1>Chart 9</h1>
      <svg id="chart-9" width="760" height="140">
        <g transform="translate(70, 70)">
          <g class="item" transform="translate(0, 0)"></g>
          <g class="item" transform="translate(120, 0)"></g>
          <g class="item" transform="translate(240, 0)"></g>
        </g>
      </svg>
    </div>
    <div class="container">
      <h1>Chart 10</h1>
      <svg id="chart-9" width="760" height="140">
        <g transform="translate(70, 70)">
          <circle r="20" style="fill: orange"></circle>
          <circle r="20" cx="120" style="fill: orange"></circle>
          <circle r="20" cx="240" style="fill: orange"></circle>
          <circle r="20" cx="360" style="fill: orange"></circle>
          <circle r="20" cx="480" style="fill: orange"></circle>
        </g>
      </svg>
    </div>
    <div class="container">
      <h1>Final</h1>
      <div id="chart-final"></div>
      <button @click="updateChartFinal">Sorted</button>
    </div>
  </div>
</template>

<script>
// import chartData from "./assets/chart.json";
import * as d3 from "d3";

export default {
  name: "Selections",
  data() {
    return {
      chart: null,
      chartData: [],
    };
  },
  components: {},
  mounted() {
    this.initChart1();

    this.initChart3();
    this.initChart4();
    this.initChart5();
    // setInterval(this.updateChart1, 1500);
    this.initChart7();
    this.initChart8();
    this.initChart9();
    this.initChartFinal();
  },
  methods: {
    initChart1() {
      d3.select("#chart-1")
        .select("g")
        .selectAll("circle")
        .style("fill", "orange")
        .attr("r", function () {
          return 10 + Math.random() * 40;
        });
      d3.select("#chart-1")
        .select("g")
        .select("circle:nth-child(3)")
        .classed("selected", true);
    },
    updateChart1() {
      d3.select("#chart-1")
        .select("g")
        .selectAll("circle")
        .transition()
        .duration(1500)
        .attr("r", function () {
          return 10 + Math.random() * 40;
        });
    },
    handleChangeChart2() {
      d3.select("#chart-2")
        .select("g")
        .selectAll("rect")
        .attr("x", (d, i) => 100 + i * 40);
    },
    initChart3() {
      d3.select("#chart-3")
        .select("g")
        .selectAll("circle")
        .data([1, 2, 3, 4, 5])
        .on("click", function (d, i) {
           d3.select("#chart-3")
            .select("g")
            .selectAll("circle")
            .style("fill", null);

          d3.select(this).style("fill", "orange");
          d3.select(".status").text("You clicked on circle " + i);
        });
    },
    initChart4() {
      d3.select("#chart-4")
        .select("g")
        .selectAll("circle")
        .data([1, 2, 3, 4, 5])
        .on("click", function () {
          d3.select("#chart-4")
            .select("g")
            .selectAll("circle")
            .style("fill", null);
          d3.select(this).style("fill", "orange");
        });
    },
    initChart5() {
      d3.select("#chart-5")
        .selectAll("g.item")
        .append("text")
        .text(function (d, i) {
          return i + 1;
        })
        .attr("x", 30)
        .attr("y", 50);
    },
    handleChangeChart6() {
      d3.select("#chart-6").selectAll("g").selectAll("circle").remove();
    },
    initChart7() {
      d3.select("#chart-7 > g")
        .selectAll("circle")
        .data([1, 2, 3, 4, 5])
        .on("click", function (d, i) {
          d3.select(".chart-7")
            .select(".status")
            .text("You clicked on circle " + i);
          console.log(i);
        });
    },
    initChart8() {
      d3.select("#chart-8 > g")
        .selectAll("circle")
        .data([1, 2, 3, 4, 5])
        .attr("r", function (d, i) {
          let odd = i % 2 === 0;
          return odd ? 40 : 20;
        })
        .style("fill", function (d, i) {
          let odd = i % 2 === 1;
          return odd ? "orange" : "blue";
        });
    },
    initChart9() {
      d3.select("#chart-9 > g")
        .selectAll("g.item")
        .data([1, 2, 3, 4, 5])
        .call(this.appendCircleAndText);
    },
    appendCircleAndText(selection) {
      selection.append("circle").attr("r", 50).style("fill", "orange");
      selection
        .append("text")
        .text(function (d, i) {
          return i + 1;
        })
        .attr("y", 50)
        .attr("x", 30);
    },
    initChartFinal() {
      let data = [
        {
          name: "Andy",
          width: "148",
        },
        {
          name: "Beth",
          width: "156",
        },
        {
          name: "Craig",
          width: "124",
        },
        {
          name: "Diane",
          width: "140",
        },
        {
          name: "Evelyn",
          width: "152",
        },
      ];
      let base = d3
        .select("#chart-final")
        .selectAll("div")
        .data(data)
        .enter()
        .append("div")
        .attr("class", "person")
        .style("display", "flex")
        .style("align-items", "center");
      base
        .append("div")
        .attr("class", "label")
        .text((d) => d.name)
        .style("width", "50px");
      base
        .append("div")
        .attr("class", "bar")
        .style("width", (d) => `${d.width}px`)
        .style("height", "20px")
        .style("background-color", "blue")
        .style("margin", "5px");
      // d3.select("#chart-final").data(data).selectAll(".person")
    },
    updateChartFinal() {
      let status = d3.select("#chart-final").classed("active");

      d3.select("#chart-final")
        .selectAll(".person")
        .sort(function (a, b) {
          return status ? b.width - a.width : a.width - b.width;
        });

      if (status) {
        d3.select("#chart-final").classed("active", false);
      } else {
        d3.select("#chart-final").classed("active", true);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
#Selections {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  .table {
    width: 100%;
    font-size: 0.9em;
    margin-bottom: 2em;
    border-collapse: collapse;
    width: 760px;
    margin: 0 auto;
  }
  .table th, .table td {
    border: 1px solid #ddd;
    padding: 8px;
    line-height: 1.5em;
    text-align: left;
}
  .table-bordered {
    border: 1px solid #ddd;
  }
  svg {
    g {
      circle {
        fill: #ddd;
        &.selected {
          stroke: #555;
          stroke-width: 2px;
        }
      }
      rect {
        fill: #ddd;
      }
    }
    &#chart-3,
    &#chart-4 {
      circle {
        cursor: pointer;
      }
      g {
          circle:hover {
            fill: orange;
          }
          circle.active {
            fill: orange;
        }
      }
      
    }
    &#chart-5 {
      circle {
        fill: orange;
      }
      g.item text {
        fill: #ddd;
        font-size: 70px;
        text-anchor: middle;
        font-weight: bold;
      }
    }
    &#chart-9 {
      circle {
        fill: orange;
      }
      g.item text {
        fill: #ddd;
        font-size: 70px;
        text-anchor: middle;
        font-weight: bold;
      }
    }
  }
  #chart-final {
    width: 760px;
    margin: 0 auto;
    .person {
      .bar {
        display: inline-block;
        background-color: red;
      }
    }
  }
  button {
    border-radius: 5px;
    border: solid 1px salmon;
    padding: 5px;
    margin: 0 5px;
  }
}
</style>

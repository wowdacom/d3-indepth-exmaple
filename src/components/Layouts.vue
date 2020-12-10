<template>
  <div class="container">
    <h1>Chart 1</h1>
    <svg id="shape1">
      <g transform="translate(5, 5)">
        <g class="links"></g>
        <g class="nodes"></g>
      </g>
    </svg>
  </div>
  <div class="container">
    <h1>Chart 2</h1>
    <svg id="shape2">
        <g></g>
    </svg>
  </div>
  <div class="container">
    <h1>Chart 3</h1>
    <svg id="shape3">
        <g></g>
    </svg>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  data() {
    return {};
  },
  mounted() {
    this.initChart1();
    this.initChart2();
  },
  methods: {
    initChart1() {
      let width = 300;
      let height = 300;
      let margin = {
        left: 20,
        right: 20,
        top: 20,
        bottom: 20,
      };
      let innerWidth = width - margin.left - margin.right;
      let innerHeight = height - margin.top - margin.bottom;
      let tree = {
        name: "A1",
        children: [
          {
            name: "B1",
            children: [
              {
                name: "C1",
                value: 100,
              },
              {
                name: "C2",
                value: 300,
              },
              {
                name: "C3",
                value: 200,
              },
            ],
          },
          {
            name: "B2",
            children: [
              {
                name: "C1",
                children: [
                  {
                    name: "C1",
                    value: 100,
                  },
                  {
                    name: "C2",
                    children: [
                      {
                        name: "C1",
                        value: 100,
                      },
                      {
                        name: "C2",
                        value: 300,
                      },
                      {
                        name: "C3",
                        value: 200,
                      },
                    ],
                  },
                ],
              },
              {
                name: "C2",
                value: 300,
              },
            ],
          },
        ],
      };

      //   let svg = d3.select("#shape1")
      let treeLayout = d3.tree().size([innerWidth, innerHeight]);

      let root = d3.hierarchy(tree);

      treeLayout(root);

      d3.select("#shape1").attr("width", width).attr("height", height);

      // Nodes

      d3.select("#shape1 g.nodes")
        .selectAll("circle.node")
        .data(root.descendants())
        .enter()
        .append("circle")
        .classed("node", true)
        .attr("cx", function (d) {
          return d.x;
        })
        .attr("cy", function (d) {
          return d.y;
        })
        .attr("r", 4);

      // Links
      d3.select("#shape1 g.links")
        .selectAll("line.link")
        .data(root.links())
        .enter()
        .append("line")
        .classed("link", true)
        .attr("x1", function (d) {
          return d.source.x;
        })
        .attr("y1", function (d) {
          return d.source.y;
        })
        .attr("x2", function (d) {
          return d.target.x;
        })
        .attr("y2", function (d) {
          return d.target.y;
        });
    },
    initChart2() {
      let data = {
        name: "A1",
        children: [
          {
            name: "B1",
            children: [
              {
                name: "C1",
                value: 100,
              },
              {
                name: "C2",
                value: 300,
              },
              {
                name: "C3",
                value: 200,
              },
            ],
          },
          {
            name: "B2",
            value: 200,
          },
        ],
      };

      let treemapLayout = d3.treemap().size([400, 200]).paddingOuter(16);

      let rootNode = d3.hierarchy(data);

      rootNode.sum(function (d) {
        return d.value;
      });

      treemapLayout(rootNode);

      d3.select("#shape2").attr("width", 400).attr("height", 200)

      let nodes = d3
        .select("#shape2 g")
        .selectAll("g")
        .data(rootNode.descendants())
        .enter()
        .append("g")
        .attr("transform", function (d) {
          return "translate(" + [d.x0, d.y0] + ")";
        });

      nodes
        .append("rect")
        .attr("width", function (d) {
          return d.x1 - d.x0;
        })
        .attr("height", function (d) {
          return d.y1 - d.y0;
        });

      nodes
        .append("text")
        .attr("dx", 4)
        .attr("dy", 14)
        .text(function (d) {
          return d.data.name;
        });

    },
  },
};
</script>

<style lang="scss">
.node {
  fill: steelblue;
  stroke: none;
}

.link {
  fill: none;
  stroke: #ccc;
  stroke-width: 1px;
}

rect {
  fill: cadetblue;
  opacity: 0.3;
  stroke: white;
}
text {
  font-family: "Helvetica Neue", Helvetica, sans-serif;
  fill: white;
  font-size: 10px;
}
</style>

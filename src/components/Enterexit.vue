<template>
  <div id="Datajoins">
    <div class="containter">
      <h1>Chart 1</h1>
      <div id="box-1">
        <div></div>
        <div></div>
        <div></div>
      </div>
      <button @click="handleUpdateChart1">
        Add elements using .enter and .append
      </button>
    </div>
    <div class="containter">
      <h1>Chart 2</h1>
      <div id="box-2">
        <div></div>
        <div></div>
        <div></div>
      </div>
    </div>
    <div class="containter">
      <h1>Chart 3</h1>
      <svg id="box-3"></svg>
      <a href="https://observablehq.com/@d3/selection-join">參考網址</a>
    </div>
    <p>D3 API 文件好複雜</p>
    <p>https://github.com/d3/d3-selection/blob/v2.0.0/README.md#selection_classed</p>
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
    setInterval(() => {
        this.initChart2();
    }, 5000);
    // this.initChart2();
    this.initChart3();
  },
  methods: {
    initChart1() {
      var myData = "hello world!",
        rand = Math.floor(Math.random() * myData.length + 1),
        enter = myData.slice(0, rand).split("");
      d3.select("#box-1")
        .selectAll("div")
        .data(enter)
        .text((d) => d)
        .style("line-height", "50px")
        .style("width", "50px")
        .style("background-color", "orange")
        .style("margin", "5px")
        .style("text-align", "center");
    },
    handleUpdateChart1() {
      var myData = "hello world!",
        rand = Math.floor(Math.random() * myData.length + 1),
        enter = myData.slice(0, rand).split("");

      //增加缺少的
      d3.select("#box-1")
        .selectAll("div")
        .data(enter)
        .enter()
        .append("div")
        .text((d) => d)
        .style("line-height", "50px")
        .style("width", "50px")
        .style("background-color", "orange")
        .style("margin", "5px")
        .style("text-align", "center");
      //移除多餘的
      d3.select("#box-1").selectAll("div").data(enter).exit().remove();
    },
    initChart2() {
      const box2 = d3.select("#box-2");

      box2
        .selectAll("div")
        .data(this.randomLetters2(), (d) => d)
        .join(
          (enter) =>
            enter
              .append("div")
              .style("background-color", "green")
              .text((d) => d)
              .text((d) => d)
              .style("line-height", "50px")
              .style("width", "50px")
              .style("background-color", "orange")
              .style("margin", "5px")
              .style("text-align", "center")
              .attr("position", "relative")
              .call((enter) => enter.transition().duration(1000)),
          (update) =>
            update
              .style("background-color", "gray")
              .style("opacity", 0)
              .style("position", "relative")
              .style("top", "50px")
              .call((update) =>
                update.transition().delay(1000).duration(1000).style("opacity", 1).style("top", "0px")
              ),
          (exit) =>
            exit
              .style("background-color", "red")
              .style("position", "relative")
              .call((exit) =>
                exit.transition().duration(1000).style("opacity", 0).style("top", "-50px").remove()
              )
        );
    },
    randomLetters2() {
      var myData = "hello world!",
        rand = Math.floor(Math.random() * myData.length + 1),
        enter = myData.slice(0, rand).split("");
      return d3.shuffle(enter);
    },
    randomLetters() {
      return d3
        .shuffle("abcdefghijklmnopqrstuvwxyz".split(""))
        .slice(0, Math.floor(6 + Math.random() * 20))
        .sort();
    },
    initChart3() {
      let width = 500;
      let svg = d3
        .select("#box-3")
        .attr("height", 33)
        .attr("width", width)
        .attr("viewBox", `0 -20 ${width} 33`);

      setInterval(() => {
        const t = svg.transition().duration(750);

        svg
          .selectAll("text")
          .data(this.randomLetters(), (d) => d)
          .join(
            (enter) =>
              enter
                .append("text")
                .style("fill", "green")
                .attr("x", (d, i) => i * 16)
                .attr("y", -30)
                .text((d) => d)
                .call((enter) => enter.transition(t).attr("y", 0)),
            (update) =>
              update
                .style("fill", "black")
                .attr("y", 0)
                .call((update) =>
                  update.transition(t).attr("x", (d, i) => i * 16)
                ),
            (exit) =>
              exit
                .style("fill", "brown")
                .call((exit) => exit.transition(t).attr("y", 30).remove())
          );

        // yield svg.node();
        // await Promises.tick(2500);
      }, 2500);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  lang="scss" scoped>
#box-1 {
  display: flex;
}
#box-2 {
  display: flex;
}
</style>

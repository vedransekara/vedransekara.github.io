---
layout: post
title: Game Of Thrones network visualization
---

I was watching the season finale of Game of Thrones the other day and wondered---with so many characters in the series what does the interaction network look like?
Well, as it turns out I was not the first person to get this thought. In fact A. Beveridge and J. Shan read through _Storm Of Swords_ (third book in the series) and mapped all the interactions between characters, and released the data. You can read more about their cool project [here](https://www.macalester.edu/~abeverid/thrones.html). They are, further, planning to release data regarding the others books as well.

While A. Beveridge and J. Shan used [Gephi](https://gephi.org/) to draw their network I wanted to do something more interactive, so chose to do it in [D3js](https://d3js.org/). I you haven't already heard of D3 you should definitely check it out! There are many great example on the D3 homepage, and I especially like Bostok force-directed network visualization [see [here](http://bl.ocks.org/mbostock/4062045)]. Being very lazy, I borrowed the code [from here](https://bl.ocks.org/mbostock/4062045), wrote a python script that modified the Game Of Thrones data into the correct format, scaled node-size according to their degree (number if connections), and assigned nice colors to each person. The result is visible below. The plot is interactive meaning you drag nodes and if you hover your cursor over a node it will display the name of the character.

<body>
  <div id="got_network"></div>
<style>

div.got_network {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
}

.node {
  stroke: black;
  stroke-width: 2px;
}

  .node:hover {
    opacity: 0.8;
    stroke: white;
  }

.link {
  stroke: black;
  stroke-opacity: .5;
}
</style>
</body>

<script src="https://d3js.org/d3.v3.min.js"></script>
<script>

var width = 800,
    height = 640;

var force = d3.layout.force()
    .charge(-120)
    .linkDistance(50)
    .gravity(0.05)
    .size([width, height]);

var svg = d3.select("div#got_network").append("svg")
    .attr("width", width)
    .attr("height", height);

d3.json("/data/got.json", function(error, graph) {
  if (error) throw error;

  force
      .nodes(graph.nodes)
      .links(graph.links)
      .start();

  var link = svg.selectAll(".link")
      .data(graph.links)
    .enter().append("line")
      .attr("class", "link")
      .style("stroke-width", function(d) { return Math.sqrt(d.value); });

  var node = svg.selectAll(".node")
      .data(graph.nodes)
    .enter().append("circle")
      .attr("class", "node")
      .attr("r", function(d) { return Math.sqrt(d.size)*2.5; })
      .style("fill", function(d) { return d.color; })
      .call(force.drag);

  node.append("title")
      .text(function(d) { return d.name; });

  force.on("tick", function() {
    link.attr("x1", function(d) { return d.source.x; })
        .attr("y1", function(d) { return d.source.y; })
        .attr("x2", function(d) { return d.target.x; })
        .attr("y2", function(d) { return d.target.y; });

    node.attr("cx", function(d) { return d.x; })
        .attr("cy", function(d) { return d.y; });
  });
});
</script>
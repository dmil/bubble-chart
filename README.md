# Bubble Chart

Sample File
https://bl.ocks.org/mbostock/4063269

Steps to adopt the sample bubble chart above.

## Step 1

create a new `index.html` File

```
<!DOCTYPE html>
<html>
<head>
  <title> Bubble Chart </title>
</head>
<body>

</body>
</html>
```

remember, the metadata and stylesheets will go in the head `<head> </head>`

while the HTML and JavaScript will go in the `<body> </body>`

## Step 2

Grab the data exactly as is from the example and save it in a file with the
correct name, in this case `flare.csv`

## Step 3

Grab the code from the sample website above, notice how it is just a code
snippet containing the chart (there is no head and body section like a
well-formed webpage should have). Put the CSS in the head and the HTML and
JavaScript in the body.

Notice that the HTML in the sample file is not well formed (there is no <head>
and <body> tag). its not a complete web-page just a code snippet. We will now
fill this code into the template from Step 1.

Your `index.html` will then look like this. I've commented the code below
with things that you should notice.

```
<!DOCTYPE html>
<html>
<head>
  <!-- This is the head where the metadata goes
  notice how we moved the styles here. -->
  <meta charset="utf-8">
  <title> Example Project </title>
  <style>

  text {
    font: 10px sans-serif;
    text-anchor: middle;
  }

  </style>
</head>

<body>

  <!-- This is the body where the content of the page goes
  notice how we moved the HTML and JavaScript here. -->

  <h1> Example Project </h1>
  <svg width="960" height="880"></svg>
  <script src="https://d3js.org/d3.v4.min.js"></script>
  <script>

  var svg = d3.select("svg"),
      width = +svg.attr("width");

  var format = d3.format(",d");

  var color = d3.scaleOrdinal(d3.schemeCategory20c);

  var pack = d3.pack()
      .size([width, width])
      .padding(1.5);

  d3.csv("data.csv", function(d) {
    d.value = +d.value;
    if (d.value) return d;
  }, function(error, classes) {
    if (error) throw error;

    var root = d3.hierarchy({children: classes})
        .sum(function(d) { return d.value; })
        .each(function(d) {
          if (id = d.data.id) {
            var id, i = id.lastIndexOf(".");
            d.id = id;
            d.package = id.slice(0, i);
            d.class = id.slice(i + 1);
          }
        });

    var node = svg.selectAll(".node")
      .data(pack(root).leaves())
      .enter().append("g")
        .attr("class", "node")
        .attr("transform", function(d) { return "translate(" + d.x + "," + d.y + ")"; });

    node.append("circle")
        .attr("id", function(d) { return d.id; })
        .attr("r", function(d) { return d.r; })
        .style("fill", function(d) { return color(d.package); });

    node.append("clipPath")
        .attr("id", function(d) { return "clip-" + d.id; })
      .append("use")
        .attr("xlink:href", function(d) { return "#" + d.id; });

    node.append("text")
        .attr("clip-path", function(d) { return "url(#clip-" + d.id + ")"; })
      .selectAll("tspan")
      .data(function(d) { return d.class.split(/(?=[A-Z][^A-Z])/g); })
      .enter().append("tspan")
        .attr("x", 0)
        .attr("y", function(d, i, nodes) { return 13 + (i - nodes.length / 2 - 0.5) * 10; })
        .text(function(d) { return d; });

    node.append("title")
        .text(function(d) { return d.id + "\n" + format(d.value); });
  });

  </script>

</body>
</html>
```

## Step 4

Split up the HTML, CSS, and JavaScript

`index.html` should look like this:

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>All the charts. All of them.</title>
  <link rel="stylesheet" href="chart1.css" media="screen">
</head>

<body>
  <h1>Our Millitary is too Big.</h1>
  <h2>Too Much Money is Being Spent on Defense</h2>

  <!-- This is the SVG element that the chart will bind to -->
  <svg width="960" height="880"></svg>

  <script src="https://d3js.org/d3.v4.min.js"></script>
  <script src="main.js"></script>
</body>
</html>
```

`styles.css` should look like this:

```
text {
  font: 10px sans-serif;
  text-anchor: middle;
}
```

`main.js` should look like this:

```
var svg = d3.select("svg"),
    width = +svg.attr("width");

var format = d3.format(",d");

var color = d3.scaleOrdinal(d3.schemeCategory20c);

var pack = d3.pack()
    .size([width, width])
    .padding(1.5);

d3.csv("flare.csv", function(d) {
  d.value = +d.value;
  if (d.value) return d;
}, function(error, classes) {
  if (error) throw error;

  var root = d3.hierarchy({children: classes})
      .sum(function(d) { return d.value; })
      .each(function(d) {
        if (id = d.data.id) {
          var id, i = id.lastIndexOf(".");
          d.id = id;
          d.package = id.slice(0, i);
          d.class = id.slice(i + 1);
        }
      });

  var node = svg.selectAll(".node")
    .data(pack(root).leaves())
    .enter().append("g")
      .attr("class", "node")
      .attr("transform", function(d) { return "translate(" + d.x + "," + d.y + ")"; });

  node.append("circle")
      .attr("id", function(d) { return d.id; })
      .attr("r", function(d) { return d.r; })
      .style("fill", function(d) { return color(d.package); });

  node.append("clipPath")
      .attr("id", function(d) { return "clip-" + d.id; })
    .append("use")
      .attr("xlink:href", function(d) { return "#" + d.id; });

  node.append("text")
      .attr("clip-path", function(d) { return "url(#clip-" + d.id + ")"; })
    .selectAll("tspan")
    .data(function(d) { return d.class.split(/(?=[A-Z][^A-Z])/g); })
    .enter().append("tspan")
      .attr("x", 0)
      .attr("y", function(d, i, nodes) { return 13 + (i - nodes.length / 2 - 0.5) * 10; })
      .text(function(d) { return d; });

  node.append("title")
      .text(function(d) { return d.id + "\n" + format(d.value); });
});
```

## Step 5

Replace the data with data from elsewhere but in the exact same format,
and you will have a working graphic!

## Notes

### Where the D3 finds the data

Notice the following line in the JavaScript
```
d3.csv("flare.csv", function(d) {
```
that line is telling you where the D3 is looking for the data

### If you have multiple charts on the same page

If you have multiple charts on the same page, I would do the following:

1. Put the code for each chart in its own distinct folder
2. Give each chart an id, for example in the HTML instead of

  ```
  <svg width="960" height="880"></svg>
  ```

  have something like this
  ```
  <svg id="chart1" width="960" height="880"></svg>
  ```

3. Make sure the CSS and JavaScript for each chart are binding to the CSS selector
  for the particular element in the HTML that you want it to bind to. For example,
  look at the CSS selector in the first line of JavaScript, instead  of

  ```
  var svg = d3.select("svg"),
  ```

  you will want

  ```
  var svg = d3.select("#chart1"),
  ```

  and in the CSS, instead of simply

  ```
  text {
    font: 10px sans-serif;
    text-anchor: middle;
  }
  ```

  you will want all of your CSS selectors to look like this

  ```
  #chart1 text {
    font: 10px sans-serif;
    text-anchor: middle;
  }
  ```

## Example with two charts on the same page

  Here is an example file structure for a single page with two charts

  ![](https://www.evernote.com/shard/s150/sh/68832346-f176-4359-ad74-def7dac5a372/b296fed046f93d98/res/5c8835c1-0eca-41b3-babe-3bca05e8f0e7/skitch.png?resizeSmall&width=832)


  This is what the `index.html` will look like, notice we added an `id` tag for each element where the chart will bind to.

  ![](https://www.evernote.com/shard/s150/sh/8d1bf083-7691-4f44-bf33-cb8183b55dbf/778e721260d0e910/res/331684f6-2d74-4b00-999d-8d6e723becb7/skitch.png?resizeSmall&width=832)


  then you have to make sure the JavaScript binds to the element with the `id` of `chart1` with the CSS Selector `#chart1` rather than just to any SVG element as is default.

  ![](https://www.evernote.com/shard/s150/sh/9579bc9f-2ef8-4940-a212-4537c93de7b2/45c485ddd13daf97/res/6343fcec-2ff5-4200-9b80-b6e095df091c/skitch.png)

  finally, make sure the CSS of each chart doesn't conflict with the other by modifying the CSS selectors in `chart1.css` and `chart2.css` like this

  ![](https://www.evernote.com/shard/s150/sh/0fd18ec0-f128-4379-9973-f2f3ac7ca5e3/3d260d1f5f8eddc4/res/b7391064-517c-4880-9afc-7f17edb3e4eb/skitch.png?resizeSmall&width=832)

  For a more complete example, you can check out the `example-project` GitHub repo (coming soon!!!!)

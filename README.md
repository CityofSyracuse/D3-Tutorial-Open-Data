![chart](https://github.com/CityofSyracuse/D3-Tutorial-Open-Data/assets/43705075/4c74fd71-ca5d-416a-be73-233366979919)

Creating a Data Visualization using JavaScript's D3 Library and a little Python for data transformation, using Open Data
Step 1: Do some data transformation in Python

* Refer to the Vacant Properties.ipynb script

Step 2: Go to Observablehq.com to host your JavaScript D3 Code
Here we are going to add the following Code.  You can make a fork of their Bubble Chart Template and modify with your code, or you can follow along with this tutorial.

Step 3:  Load your CSV into your Observable Notebook

Step 4:  Create a Chart and specify the sizes and set a variable Owner and assign it to the data from the "Owner" column of the loaded CSV.

chart = {
  // Specify the dimensions of the chart.
  const width = 928;
  const height = width;
  const margin = 1; // to avoid clipping the root circle stroke
  const Owner = d => d.Owner;

Step 5: Specify formatting and color scales

  // Specify the number format for values.
  const format = d3.format(",d");

  // Create a categorical color scale.
  const color = d3.scaleOrdinal(d3.schemeTableau10);

  // Create the pack layout.
  const pack = d3.pack()
      .size([width - margin * 2, height - margin * 2])
      .padding(3);

  // Compute the hierarchy from the (flat) data; expose the values
  // for each node; lastly apply the pack layout.
  const root = pack(d3.hierarchy({children: data})
         .sum(d => d.Property_Count));

Step 6: Create an SVG Container

  // Create the SVG container.
  const svg = d3.create("svg")
      .attr("width", width)
      .attr("height", height)
      .attr("viewBox", [-margin, -margin, width, height])
      .attr("style", "max-width: 100%; height: auto; font: 10px sans-serif;")
      .attr("text-anchor", "middle");

Step 7: Create the bubble nodes

    // Place each (leaf) node according to the layout’s x and y values.
    const node = svg.append("g")
      .selectAll()
      .data(root.leaves())
      .join("g")
        .attr("transform", d => `translate(${d.x},${d.y})`);
        
Step 8:  Create a tooltip taht will appear on Hover.  If you want to modify colors, you can adjust those values here

  // Add a tooltip div to the body for displaying owner information
  const tooltip = d3.select("body").append("div")
      .attr("class", "tooltip")
      .style("position", "absolute")
      .style("visibility", "hidden")
      .style("background-color", "white")
      .style("border", "1px solid #ccc")
      .style("padding", "5px")
      .style("border-radius", "3px")
      .style("font", "12px sans-serif");

Step 9:  Here we will create filled circles for the bubbles.  We are also creating an event listener which on "mousover will trigger some HTML code to be displayed with the name of the owner and the number of properties that this owner has in a pop up.

    // Add a filled circle.
    node.append("circle")
        .attr("fill-opacity", 0.7)
        .attr("fill", d => color(d.data))
        .attr("r", d => d.r)
        .on("mouseover", function(event, d) {
          tooltip.style("visibility", "visible")
            .html( "<b>" + "Owner: " + "</b>" + d.data.Owner  + "<br/>" + "<b>" + "Num. Properties: " + "</b>" + d.data.Property_Count);
        })
        .on("mousemove", function(event) {
          tooltip.style("top", (event.pageY - 10) + "px")
            .style("left", (event.pageX + 10) + "px");
        })
        .on("mouseout", function() {
          tooltip.style("visibility", "hidden");
        });

Step 10:  Making a label for each obble and having this display in a "tspan".  If you would like other information to show inside of the bubble, just modify what is in the brackets where it currently states [d.data.Owner].

    // Add a label.
    const text = node.append("text")
        .attr("clip-path", d => `circle(${d.r})`);
  
    // Add a tspan
    text.selectAll()
        .data(d => [d.data.Owner])
        .join("tspan")
        .attr("x", 0)
        .attr("y", (d, i) => i * 12) // Adjust the y position for multiple lines
        .text(d => d);
  
    return Object.assign(svg.node(), {scales: {color}});
  }

Completed Code for both the Python Script and the D3 Code can be viewed HERE.









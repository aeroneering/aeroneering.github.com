---
layout: post
title: Plotting airfoils with D3.js and introducing AirfoilJSON
---
One of the things I've been learning about lately is 2D airfoil design.  One of the things I've been wanting to do is to plot some of the airfoils
from the [UIUC Airfoil Coordinates Database](http://www.ae.illinois.edu/m-selig/ads/coord_database.html).  It's also a great excuse to learn more about [D3.js](http://d3js.org/) and tinker with it a little.

The result of a little yak-shaving is a JSON-based spec for serializing airfoil data similar to what you find at the UIUC Airfoil Coordinates Database.  The spec itself is designed to be easy to read and implement, and is inspired by the (IMHO) awesome [GeoJSON specification](http://www.geojson.org/geojson-spec.html).  Here's an example of a minimal [AirfoilJSON](/airfoiljson/) JSON object:

{% highlight javascript %}
{
    "type" : "Airfoil",
    "name" : "A very flat plate with some thickness",
    "description" : "This member is not specified as part of AirfoilJSON but is valid because AirfoilJSON may have any number of members.",
    "coordinates" : [
        {"x" : 1.000000, "y" : 0.100000},
        {"x" : 0.000000, "y" : 0.100000},
        {"x" : 0.000000, "y" : -0.100000},
        {"x" : 1.000000, "y" : -0.100000}
    ],
    "uses" : "This plate may not good for containing food, but it is useful in explaining the basics of aerodynamics."
}
{% endhighlight %}

The end goal with [AirfoilJSON](/airfoiljson/) was to make visualizing an airfoil with D3 as easy as possible.  Here's an example of using D3 to render a NACA 2412 airfoil converted [from the UIUC database](http://www.ae.illinois.edu/m-selig/ads/coord/naca2412.dat) [to AirfoilJSON](/airfoil.js/naca2412.json).

{% highlight javascript %}
// Begin when the document is ready.
$(document).ready(function() {
	// Load AirfoilJSON via Ajax.
	$.getJSON("/data/airfoils/naca2412.json", function(data) {
		// Set up our basic SVG sizes.  I'd like to make this responsive eventually.
		var width = 750,
			height = 250,
			padding = 10;

		// Calculate the min and max of our X coordinates.
		var extentX = d3.extent(data.coordinates, function(obj) {
			return obj["x"];
		});

		// Calculate the min and max of our Y coordinates.
		var extentY = d3.extent(data.coordinates, function(obj) {
			return obj["y"];
		});

		// Calculate the scale, domain, and range of our X axis.
		var xScale = d3.scale.linear()
			.domain(extentX)
			.range([0, width - padding], extentX.max);

		// Calculate the scale, domain, and range of our Y axis.
		var yScale = d3.scale.linear()
			.domain(extentY)
			// Note that we're playing with the range a bit here so the airfoil doesn't fill the SVG document.
			.range([height / 2 - padding, 0]);

		// Create an SVG line using the X and Y values from our AirfoilJSON response.
		var line = d3.svg.line()
			.x(function(d) {
				return xScale(d["x"]);
			})
			.y(function(d) {
				return yScale(d["y"]);
			})
			.interpolate("basis");

		// Bring everything together.
		d3.select("#airfoil")
			.append("svg") // Append to the placeholder div with our size information.
			.attr("width", width)
			.attr("height", height)
			.attr("id", "airfoilsvg") // Give this an id in case we want to reference it later.
			.append("g") // Add an SVG group.
			.append("svg:path") // Add an SVG path.
			.datum(data.coordinates) // Use our coordinates to set the bounds.
			.attr("class", "line") // Useful for styling.
			.attr("d", line); // Append our line we created earlier as SVG.
		d3.select("#airfoil")
			.append("p")
			.text(data.name + " airfoil"); // Append the name of the airfoil from our AirfoilJSON response.
	});
});
{% endhighlight %}

Here's the NACA 2412 airfoil, found at the root of many smaller Cessna aircraft:

<div id="airfoil"></div>
<script type="text/javascript">
$(document).ready(function() {
	$.getJSON("/data/airfoils/naca2412.json", function(data) {
		var width = 750,
			height = 250,
			padding = 10;

		var extentX = d3.extent(data.coordinates, function(obj) {
			return obj["x"];
		});

		var extentY = d3.extent(data.coordinates, function(obj) {
			return obj["y"];
		});

		var xScale = d3.scale.linear()
			.domain(extentX)
			.range([0, width - padding], extentX.max);

		var yScale = d3.scale.linear()
			.domain(extentY)
			.range([height / 2 - padding, 0]);

		var line = d3.svg.line()
			.x(function(d) {
				return xScale(d["x"]);
			})
			.y(function(d) {
				return yScale(d["y"]);
			})
			.interpolate("basis");

		d3.select("#airfoil")
			.append("svg")
			.attr("width", width)
			.attr("height", height)
			.attr("id", "airfoilsvg")
			.append("g")
			.append("svg:path")
			.datum(data.coordinates)
			.attr("class", "line")
			.attr("d", line);
		d3.select("#airfoil")
			.append("p")
			.text(data.name + " airfoil");
	});
});
</script>

This is obviously just scratching the surface, but was definitely a lot of fun and a great start.  There's a lot more information about D3 at [d3js.org](http://d3js.org/).  [This JSFiddle](http://jsfiddle.net/Q4Mfc/1/) also helped get me started creating a line.  The [D3 Workshop](http://bost.ocks.org/mike/d3/workshop/) is also a great collection of demos and information.

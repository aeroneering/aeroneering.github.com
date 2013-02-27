---
layout: page
title: airfoil.js
---


<!-- <script type="text/javascript" src="./airfoil.js"> -->
<!-- http://jsfiddle.net/Q4Mfc/1/ -->
<script type="text/javascript">
$(document).ready(function() {
	$.getJSON("naca2412.json", function(data) {
		var width = 750,
			height = 300,
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
			});

		d3.select("#airfoil")
			.append("svg")
			.attr("width", width)
			.attr("height", height)
			.attr("id", "airfoilsvg")
			.append("g")
			.attr("transform", "translate(5,5)")
			.append("svg:path")
			.datum(data.coordinates)
			.attr("class", "line")
			.attr("d", line);
		d3.select("h2")
			.append("h3")
			.text(data.name);
	});
});

</script>
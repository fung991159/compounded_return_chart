<style>
.chart {
    display: block;
    margin-left: auto;
    margin-right: auto;
}
</style>
<html lang="en">
	<head>
		<meta charset="utf-8">
		<title>D3: SVG bar chart with value labels (centered)</title>
		<script type="text/javascript" src="../d3.js"></script>
		<style type="text/css">
			/* No style rules here yet */		
		</style>
	</head>
	<body>
		<script type="text/javascript">
			//Width and height
			var w = 500;
			var h = 100;
			var barPadding = 1;
			
			d3.csv("http://localhost/d3project/time_scale_data.csv", rowConverter, function(data) {
			
			//Create SVG element
			var svg = d3.select("body")
						.append("svg")
						.attr("width", w)
						.attr("height", h)
						.attr("viewBox=", "0 0 960 500")
						.attr("preserveAspectRatio", "xMidYMid meet");
						
			svg.selectAll("rect")
			   .data(dataset)
			   .enter()
			   .append("rect")
			   .attr("x", function(d, i) {
			   		return i * (w / dataset.length);
			   })
			   .attr("y", function(d) {
			   		return h - (d * 4);
			   })
			   .attr("width", w / dataset.length - barPadding)
			   .attr("height", function(d) {
			   		return d * 4;
			   })
			   .attr("fill", function(d) {
					return "rgb(0, 0, " + Math.round(d * 10) + ")";
			   });
			svg.selectAll("text")
			   .data(dataset)
			   .enter()
			   .append("text")
			   .text(function(d) {
			   		return d;
			   })
			   .attr("text-anchor", "middle")
			   .attr("x", function(d, i) {
			   		return i * (w / dataset.length) + (w / dataset.length - barPadding) / 2;
			   })
			   .attr("y", function(d) {
			   		return h - (d * 4) + 14;
			   })
			   .attr("font-family", "sans-serif")
			   .attr("font-size", "11px")
			   .attr("fill", "white");
            });
		</script>
	</body>
</html>

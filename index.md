<style>
.chart {
    display: block;
    margin-left: auto;
    margin-right: auto;
}
</style>
<head>
        <meta charset="utf-8">
        <title>Power of compounded interest, the world 8th wonder!</title>
        <script type="text/javascript" src="https://d3js.org/d3.v5.min.js"></script>
        <style type="text/css">
		</style>
    </head>
    <body>
	<div class = "dataInput">
		<label>assume return rate (%)</label> <input id="returnRate" type="text	" name="returnRate" onkeyup="UpdateValue()" value ="10"><br>
		<label>Year of investment</label> <input id="yearOfInvestment" type="text" name="yearOfInvestment" onkeyup="UpdateValue()" value = "30"><br>
		<label>Initial sum</label> <input id="initialSum"type="text" name="initialSum" onkeyup="UpdateValue()" value ="1000"><br>
	</div>

	<script type="text/javascript">
		var inputReturnRate = 10
		var yearOfInvestment = 30
		var initialSum = 1000

		var dataset

		function calculator() {   
			var endingInvestment = initialSum;
			// var endingInvestment = initialSum*(1+inputReturnRate/100) 
			dataset = [];
			dataset.push({year: 0, endingInvestment: endingInvestment})
			for (var i=1; i<=yearOfInvestment; i++){
				endingInvestment = endingInvestment*(1+inputReturnRate/100)^(i)
				dataset.push({
					year: i,
					endingInvestment: +endingInvestment
				})
			};
		}
		calculator();  //run for initial chart
		//Width and height
		var w = 800;
		var h = 300;
		var padding = 40;
		
		xScale = d3.scaleLinear().domain([0,yearOfInvestment]).range([padding, w-10]).nice() ;
		yScale = d3.scaleLinear().domain(d3.extent(dataset, (d)=> {return +d.endingInvestment})).range([h - padding, 0]);
	
		//Create scale functions
		// xScale = d3.scaleTime().domain([0,yearOfInvestment]).range([padding, w]);

						
		//Define axes
		xAxis = d3.axisBottom()
				  .scale(xScale)

		//Define Y axis
		yAxis = d3.axisLeft()
					.scale(yScale)
		
		//Line generator
		var line = d3.line()
					 .x((d) => {return xScale(d.year);})
					 .y((d) => {return yScale(d.endingInvestment);});

		// Create SVG element
		var svg = d3.select("body")
					.append("svg")
					.attr("width", w)
					.attr("height", h)
					.attr("viewBox=", "0 0 960 500")
					.attr("preserveAspectRatio", "xMidYMid meet");
		//Draw line
		var Path = svg.append("path")
						.datum(dataset)
						.attr("class", "line")
						.attr("d", line)
						.style("fill", "none")
						.style("stroke", "darkgreen");

		//Draw axes
		svg.append("g")
						.attr("class", "xAxis")
						.attr("transform", "translate(0," + (h - padding) + ")")
						.call(xAxis);
			
		svg.append("g")
						.attr("class", "yAxis")
						.attr("transform", "translate(" + padding + ",0)")
						.call(yAxis);
		
		//update javascript variable on html input

		var timeout = null;
		function UpdateValue() {
			clearTimeout(timeout);
			// set delay to wait for user complete input
			timeout = setTimeout(function () {
			console.log({inputReturnRate, yearOfInvestment, initialSum})
			console.log(dataset)
				inputReturnRate = document.getElementById("returnRate").value
				yearOfInvestment = document.getElementById("yearOfInvestment").value
				initialSum = document.getElementById("initialSum").value
				updateD3Chart();
    		}, 500);

		} 
		 
		//update chart according to new value
		function updateD3Chart() {

			calculator(); //recalculate new dataset

			console.log({inputReturnRate, yearOfInvestment, initialSum})
			console.log(dataset)
			
		
			//Update with new data
			var svg = d3.select("body").transition();
			xScale.domain([0,yearOfInvestment]);
			yScale.domain(d3.extent(dataset, (d)=> {return +d.endingInvestment}));

			svg.select(".line")
				.duration(750)
				.attr('d', line(dataset));
			svg.select(".xAxis")
				.duration(750)
				.call(xAxis);
			svg.select(".yAxis")
				.duration(750)
				.call(yAxis);
			}

		// updateD3Chart()  // just for testing purpose
		</script>
	</body>
</html>



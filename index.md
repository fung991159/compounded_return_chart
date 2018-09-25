
	
<head>
        <meta charset="utf-8">
        <title>Power of compounded interest, the world 8th wonder!</title>
        <script type="text/javascript" src="https://d3js.org/d3.v5.min.js"></script>
        <style type="text/css">
		</style>
    </head>
    <body>

	<div class = "dataInput">
		<label>assume return rate (%)</label> <input id="returnRate" type="text" name="returnRate" onkeyup="UpdateValue()" value ="10"><br>
		<label>Year of investment</label> <input id="yearOfInvestment" type="text" name="yearOfInvestment" onkeyup="UpdateValue()" value = "30"><br>
		<label>Initial sum</label> <input id="initialSum" type="text" name="initialSum" onkeyup="UpdateValue()" value ="1000"><br>
	</div>
	<div id = "chartContainer"></div>
	
	<!-- <img src="http://www.theinvestmentmania.com/wp-content/uploads/2016/06/8thWonder-Compounding-Interest.jpg" 
	alt="power of compounded interest, the world eighth wonder"
	width=50%> -->
	<style>
		.line {
			stroke-width: 2;
		}

		.body {
			font-size:3em;
		}
	</style>


	<script type="text/javascript">
	
	
		var inputReturnRate = 10,
			yearOfInvestment = 30,
			initialSum = 1000,
			dataset,
			endingInvestment,
		    formatValue = d3.format(".2s"), //format millions to M
			twoX_Year,
			fiveX_Year,
			tenX_Year;  
		
		function calculator() {  
			twoX_Year = Math.log((initialSum*2)/initialSum)/Math.log(1+inputReturnRate/100);
			fiveX_Year = Math.log((initialSum*5)/initialSum)/Math.log(1+inputReturnRate/100);
			tenX_Year = Math.log((initialSum*10)/initialSum)/Math.log(1+inputReturnRate/100); 
			dataset = [];
			dataset.push({year: +0, endingInvestment: +initialSum})
			for (var i=1; i<=yearOfInvestment; i++){
				endingInvestment = Math.round(initialSum*Math.pow((1+inputReturnRate/100),i))
				dataset.push({
					year: i,
					endingInvestment: +endingInvestment
				})
			};	
		}

		calculator();  //run for initial chart
		

		
		//Width and height
		var margin = {top: 40, right: 80, bottom: 50, left: 40},
            width = 780 - margin.left - margin.right,
            height = 300 - margin.top - margin.bottom
		
		xScale = d3.scaleLinear().domain([0,yearOfInvestment]).range([0, width-10]).nice() ;
		yScale = d3.scaleLinear().domain(d3.extent(dataset, (d)=> {return +d.endingInvestment})).range([height, 0]).nice();

		//Define axes
		xAxis = d3.axisBottom()
				  .scale(xScale);
		
		//Define Y axis
		yAxis = d3.axisLeft()
					.scale(yScale)
					.ticks(7)
					.tickFormat((d)=> { return "$" + formatValue(d) });
		
		//Line generator
		var line = d3.line()
					 .x((d) => {return xScale(d.year);})
					 .y((d) => {return yScale(d.endingInvestment);});
		
		// Create SVG element
		var svg = d3.select("#chartContainer").append("svg")
   			 .attr("width", width + margin.left + margin.right)
   			 .attr("height", height + margin.top + margin.bottom)
 			 .append("g")
			 .attr("transform", "translate(" + margin.left + "," + margin.top + ")")
			 .attr("viewBox", "0 0 780 300")
			 .attr("preserveAspectRatio", "xMidYMid meet");
		
		
		var focus = svg.append("g").style("display", "none")
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
			.attr("transform", "translate(0," + (height) + ")")
			.call(xAxis)

			
		svg.append("g")
			.attr("class", "yAxis")
			.call(yAxis);
		
		
		// text label for the x axis
		svg.append("text")             
			.attr("transform", "translate(" + (width/2) + " ," + (height+margin.bottom-10)+")")
			.style("text-anchor", "middle")
			.text("Year invested");
		
		// // add 2x, 5x, 10x label along the path if available
		// if (yearOfInvestment >= twoX_Year) {
		// 	svg.append("text")
		// 		.attr("class", "2x")
		// 		.attr("transform", "translate(" + xScale(twoX_Year)+ ", " + yScale(initialSum*2) + ")")
		// 		.attr("dy", "-1.5em")
		// 		.text("2x");	
		// 	svg.append("text")
		// 		.attr("class", "2x")
		// 		.attr("transform", "translate(" + xScale(twoX_Year)+ ", " + yScale(initialSum*2) + ")")
		// 		.attr("dy", "-0.3em")
		// 		.text("▼");
		// } 
		// if (yearOfInvestment >= fiveX_Year) {
		// 	svg.append("text")
		// 		.attr("class", "5x")
		// 		.attr("transform", "translate(" + xScale(fiveX_Year)+ ", " + yScale(initialSum*5) + ")")
		// 		.attr("dy", "-1.5em")
		// 		.text("5x");	
		// 	svg.append("text")
		// 		.attr("class", "5x")
		// 		.attr("transform", "translate(" + xScale(fiveX_Year)+ ", " + yScale(initialSum*5) + ")")
		// 		.attr("dy", "-0.3em")
		// 		.text("▼");
		// }
		// if (yearOfInvestment >= tenX_Year) {
		// 	svg.append("text")
		// 		.attr("class", "10x")
		// 		.attr("transform", "translate(" + xScale(tenX_Year)+ ", " + yScale(initialSum*10) + ")")
		// 		.attr("dy", "-1.5em")
		// 		.text("10x");	
		// 	svg.append("text")
		// 		.attr("class", "10x")
		// 		.attr("transform", "translate(" + xScale(tenX_Year)+ ", " + yScale(initialSum*10) + ")")
		// 		.attr("dy", "-0.3em")
		// 		.text("▼");
		// }

  		 // append the x line
  		focus.append("line")
        .attr("class", "x")
        .style("stroke", "blue")
        .style("stroke-dasharray", "3,3")
        .style("opacity", 0.5)
        .attr("y1", 0)
		.attr("y2", height);
		
		// append the y line
		focus.append("line")
        .attr("class", "y")
        .style("stroke", "blue")
        .style("stroke-dasharray", "3,3")
        .style("opacity", 0.5)
        .attr("x1", width)
        .attr("x2", width);

    	// append the circle at the intersection
   		focus.append("circle")
        .attr("class", "y")
        .style("fill", "none")
        .style("stroke", "blue")
		.attr("r", 4);
	
		// place the value at the intersection
		focus.append("text")
        .attr("class", "y1")
        .style("stroke", "white")
        .style("stroke-width", "3.5px")
        .style("opacity", 0.8)
        .attr("dx", 8)
        .attr("dy", "-1em");
  		focus.append("text")
        .attr("class", "y2")
        .attr("dx", 8)
		.attr("dy", "-1em");
		
    // place the date at the intersection
   		 focus.append("text")
        .attr("class", "y3")
        .style("stroke", "white")
        .style("stroke-width", "3.5px")
        .style("opacity", 0.8)
        .attr("dx", 8)
        .attr("dy", "-2.3em");
   		 focus.append("text")
        .attr("class", "y4")
        .attr("dx", 8)
		.attr("dy", "-2.3em");

    // append the rectangle to capture mouse
    svg.append("rect")
        .attr("width", width)
        .attr("height", height)
        .style("fill", "none")
        .style("pointer-events", "all")
        .on("mouseover", function() { focus.style("display", null); })
        .on("mouseout", function() { focus.style("display", "none"); })
		.on("mousemove", mousemove);
	
	var bisectDate = d3.bisector(function(d) { return d.year; }).left;
    function mousemove() {
		var x0 = xScale.invert(d3.mouse(this)[0]),   //use chart x value to find actualy data value
		    i = bisectDate(dataset, x0, 1),
		    d0 = dataset[i - 1],
		    d1 = dataset[i],
		    d = x0 - d0.year > d1.year - x0 ? d1 : d0;
		
		//move the circle in place
		focus.select("circle.y")
		    .attr("transform",
		          "translate(" + xScale(d.year) + "," +
		                         yScale(d.endingInvestment) + ")");

		focus.select("text.y1")
		    .attr("transform",
		          "translate(" + xScale(d.year) + "," +
		                         yScale(d.endingInvestment) + ")")
		    .text(d.endingInvestment);

		focus.select("text.y2")
		    .attr("transform",
		          "translate(" + xScale(d.year) + "," +
		                         yScale(d.endingInvestment) + ")")
		    .text("$" + d.endingInvestment);

		focus.select("text.y3")
		    .attr("transform",
		          "translate(" + xScale(d.year) + "," +
		                         yScale(d.endingInvestment) + ")")
		    .text("Year " + d.year);

		focus.select("text.y4")
		    .attr("transform",
		          "translate(" + xScale(d.year) + "," +
		                         yScale(d.endingInvestment) + ")")
		    .text("Year " + d.year);

		focus.select(".x")
		    .attr("transform",
		          "translate(" + xScale(d.year) + "," +
		                         yScale(d.endingInvestment) + ")")
		               .attr("y2", height - yScale(d.endingInvestment));

		focus.select(".y")
		    .attr("transform",
		          "translate(" + width * -1 + "," +
		                         yScale(d.endingInvestment) + ")")
		               .attr("x2", width + xScale(d.year));
	}
		
			//create summary text under chart
		var moneyFormat = d3.format("($,.0f"),  
			endingYear = dataset.length -1,
			investmentReturnMultiple = dataset[endingYear].endingInvestment/initialSum
			endingInvestment = moneyFormat(dataset[dataset.length-1].endingInvestment);
			
			p = document.createElement("p");
			p.innerHTML = "At the end of " + endingYear +" years $" + initialSum+ " initial investment becomes " + endingInvestment +
						  " which is " + Math.round(investmentReturnMultiple*10)/10  + "x of intial investment!"
			document.body.appendChild(p);

		//update javascript variable on html input
		var timeout = null;
		function UpdateValue() {
			clearTimeout(timeout);
			// set delay to wait for user complete input
			timeout = setTimeout(function () {
				inputReturnRate = document.getElementById("returnRate").value
				yearOfInvestment = document.getElementById("yearOfInvestment").value
				initialSum = document.getElementById("initialSum").value
				updateD3Chart();
    		}, 500);

		} 
		 
		//update chart according to new value
		function updateD3Chart() {

			calculator(); //recalculate new dataset
		
			//Update with new data
			var svg = d3.select("body").transition();
			xScale.domain([0,yearOfInvestment]);
			yScale.domain(d3.extent(dataset, (d)=> {return +d.endingInvestment})).nice();
			
			svg.select(".line")
				.duration(750)
				.attr('d', line(dataset));
			svg.select(".xAxis")
				.duration(750)
				.call(xAxis);
			svg.select(".yAxis")
				.duration(750)
				.call(yAxis)


			//Update Summary Text
			endingInvestment = moneyFormat(dataset[dataset.length-1].endingInvestment);
			endingYear = dataset.length -1;
			investmentReturnMultiple = dataset[endingYear].endingInvestment/initialSum;
			p.innerHTML = "At the end of " + endingYear +" years $" + initialSum+ " initial investment will becomes " + endingInvestment +
						  " which is " + Math.round(investmentReturnMultiple*10)/10 + "x of intial investment!"
			}
			
			
		</script>
	</body>



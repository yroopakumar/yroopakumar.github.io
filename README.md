Output Link
=======================

https://yroopakumar.github.io/

What Is This?
-------------

This set of different locations 5 days weather forcast. By default we will show bangalore location 5 daysweather forecast and based selction it will show waether accordingly. Here we are showing only India cities.

How it works and Source code
-----------------------------

1) Here we have two different functions getCities(); and loadWeather(); .  getCities(); in this function we are calling city.list.json from ther by default selecting Bangalore location and will show bangalore 5 days weather forecast. 

2) loadWeather(); In this function we are calling two different methods successCall,errorCall. If cityId>0 condition will accept success call function we are calling . In Success call based on api we are taking 5 days weather forecast for selected city. Otherwise by deafault we will Bangalore location.


<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="description" content="">
    <meta name="author" content="">
    <link rel="icon" href="favicon.ico">
    <title>Weather Forecast</title>
    <!-- Bootstrap core CSS -->
    <link href="dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Custom styles for this template -->
    <link href="pricing.css" rel="stylesheet">
  </head>
  <body>
  	<div style=" padding: 2em;">
		    <div class="pricing-header px-3 py-3 pt-md-5 pb-md-4 mx-auto text-center"  style="background: url(images/weather.svg) no-repeat center #1f567c;background-size: cover; border:1px solid #ddd; border-radius: 5px;">
		      <h1 class="display-4">Weather Forecast</h1>
		      <p class="lead">5 day forecast is available at any location or city</p>
			  <h3 id="city"> </h3>
				  <select class="custom-select custom-select-lg mb-3" id="sel_cities" onchange="cityChanged()">
				  </select>
		   

		    <div class="container" style="padding: 2em;">
		      <div class="card-deck mb-3 text-center" id="div_weather" >

		      </div>
		    </div>
 </div>
	</div>
    <!-- Bootstrap core JavaScript
    ================================================== -->
    <!-- Placed at the end of the document so the pages load faster -->
	<script
  src="https://code.jquery.com/jquery-3.3.1.min.js"
  integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8="
  crossorigin="anonymous"></script>
    <script src="assets/js/vendor/popper.min.js"></script>
    <script src="dist/js/bootstrap.min.js"></script>
    <script src="assets/js/vendor/holder.min.js"></script>
	<script src="moment.min.js"></script>
    <script>
      Holder.addTheme('thumb', {
        bg: '#55595c',
        fg: '#eceeef',
        text: 'Thumbnail'
      });
	  function callAjaxJson(url, dataToSend, _success, _error) {
			var _successhandler = function (response) {
				 _success(response);
			};
			var _errorhandler = function (response) {
				 _error(response);
			};
			$.ajax({
				type: "GET",
				url: url,
				//data: dataToSend,
				//contentType: "application/json; charset=utf-8",
				//dataType: "json",
				success: _successhandler,
				error: _errorhandler
			});
		}
	   
		function loadWeather(cityId){
			var key="be22248e27399bf8687078729acb5ae0";
			if(cityId>0){
			callAjaxJson("https://api.openweathermap.org/data/2.5/forecast?id="+cityId+"&APPID="+key+"&units=metric",null,successCall,errorCall);}
			else{
			var city="bangalore,IN";
			callAjaxJson("https://api.openweathermap.org/data/2.5/forecast?q="+city+"&APPID="+key+"&units=metric",null,successCall,errorCall);
			}
		}
		function successCall(output){
		if(output!=undefined && output.cod=="200"){
			console.log(output.city,name + " "+ output.city.country);
			$("#city").html(output.city.name + " "+ output.city.country);
				var count=0;
				var divdate="";
				var curDate=0;
				var curHour = 0;
				$.each(output.list, function( index, value ) {
				var hh= new Date(value.dt*1000);
				
				if(hh.getUTCDate()!=curDate && ( curHour==0 || hh.getUTCHours()==curHour)){
					console.log(value);
					if(curHour==0)
					{curHour=hh.getUTCHours();}
					console.log("-----------------");
					console.log(hh);
					console.log(hh.getUTCDate());
					console.log(hh.getUTCHours());
					console.log(curDate);
					console.log(curHour);
					console.log(hh.getUTCDate()!=curDate);
					console.log("-----------------");
					curDate=hh.getUTCDate();
					count++;
					if(count<=5){
					divdate=divdate+"<div class='card mb-2 box-shadow'>"
							+"<div class='card-header'>"
							+"<h5 class='my-0 font-weight-normal'>"+ moment(hh).format("DD-MM-YYYY") +" <small class='text-muted'> "+ moment(hh).format("HH:mm") +"</small></h5>"
							+"</div>"
							+"<div class='card-body'>"
							+"<h5 class='card-title pricing-card-title'>"+value.main.temp+" <small class='text-muted'> celsius</small></h5>"
							+"<img src='https://openweathermap.org/img/w/"+value.weather[0].icon+".png' alt='"+value.weather[0].description+"' title='"+value.weather[0].description+"'></img>"
							+"<ul class='list-unstyled mt-3 mb-4'>"
							+"<li ><strong>Max : </strong><small><b>" + value.main.temp_max+ "</b></small></li>"
							+"<li><strong>Min : </strong><small><b>" + value.main.temp_min + "</b></small> </li>"
							// +"<li>" + value.main.humidity+ " Humidity</li>"
							// +"<li>" + value.main.pressure+ " Pressure</li>"
							+"</ul>"
							+"</div>"
							+"</div>";
					}	
				}
			});
			if(divdate.length>0){
				$("#div_weather").html(divdate);
			}
		}else{
			alert("Ops.. Something went wrong");
		}
			
		}
		
		function errorCall(output){
			alert(output.responseJSON.message);
		};
		
		function getCities(){
		
			$.getJSON('https://raw.githubusercontent.com/yroopakumar/yroopakumar.github.io/master/current.city.list.json',function(data){
			
			data = jQuery.grep(data, function( a ) {
			  return a.country=="IN";
			});
			console.log("-----------------");
			console.log(data);
			console.log("-----------------");
			var output = '';  
			var sortedData= data.sort(function(a, b){
				console.log(a.name);
				var a1= a.name.toLowerCase(); b1= b.name.toLowerCase();
				if(a1== b1) return 0;
				return a1> b1? 1: -1;
			});
			$.each(sortedData, function(key,val){
			if(val.name.toLowerCase()=="bangalore")
			  output += '<option value="'+ val.id+'" selected>'+ val.name + " (" + val.country+ ')</option>';
			  else
			  output += '<option value="'+ val.id+'">'+ val.name + " (" + val.country+ ')</option>';
			});
			
			$('#sel_cities').html(output);
			});
		}
		function cityChanged(){
		 var x = document.getElementById("sel_cities").value;
		 loadWeather(x);
		}
		$( document ).ready(function() {
			getCities();
			loadWeather();
		});
    </script>
  </body>
</html>



Mapping in Minutes
==================

Good afternoon. To quickly introduce myself my name is Simon Raper. I'm statistician (or if you like data scientist) based in London and I run a data science services company called Coppelia. We've clients across many industry sectors and we do all sorts of things from machine learning to agent based modelling, to visualisation and of course mapping - which is how I got involved in the Trussell project. Thanks to Giles and Andy you already know what we did and why we did it. I'm going show you this afternoon how we did it.

It's going to be hands on if you want it to be. If you have a laptop and you'd like to work through the examples I've set things up so you can do so easily by copying and pasting code from the Coppelia website. The URL you'll need is 

Otherwise you can just listen if you'd prefer. Every now and again we'll have a pause so that people on laptops can catch up or get a bit of technical assistance from Richard or me.

I promised that I would show you how to create a beautiful interactive maps with just a few lines of javascript code. I've mislead you a little as the learning curves gets bumpier as we go on but it is true that there are some very very quick wins early on. 

If you want a few days with your feet on your desk you could present you boss with the following and tell him it took you weeks!

Here it is (as borrowed from the leaflet.js website)


	<head>
	    <script src="http://cdn.leafletjs.com/leaflet/v0.7.7/leaflet.js"></script>
	    <link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet/v0.7.7/leaflet.css" />
	    <style> #mapid {
	        height: 500px;
	    } </style>
	</head>
	<body>
	
	<div id="mapid"></div>
	
	<script>
	
	    var mymap = L.map('mapid').setView([51.505, -0.09], 13);
	
	    L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}', {
	        attribution: 'Map data &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors, <a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="http://mapbox.com">Mapbox</a>',
	        maxZoom: 18,
	        id: 'mapbox.streets-basic',
	        accessToken: your_token
	    }).addTo(mymap);
	
	</script>
	
	</body>


You'll need to substitute in this access code

	pk.eyJ1Ijoic2ltb25yYXBlciIsImEiOiJjaXBncHFob2UwMDBvdnJudHZqajA5bmJxIn0.LfOyy3BUvPeFTNPqyz9iNQ

Open a file containing this code in a web browser and incredibly this gives you a map of the entire world that is zoomable to an astonishing degree. 


I realise that perhaps some of you are not familiar with javascript so by way of a quick explanation it is a programming language, that is a set of instructions, which browsers (chrome, safari etc) can process. 

Because we all have browsers on our machines this means it is a wonderfully accessible programming language. You need only to write this code in a text file, save it, give it the file extension .htm and click on it and you'll get this result.

[For those who are following this, open a text editor, paste in the code which you'll find online, save as example_1.htm and then open in a browser]

The javascript is itself contained within another kind of script which you'll all know of even if you aren't overly familiar with how it works, that is html. (see the tags in the html above.)

Being able to express your ideas in these languages is a skill which I don't think anyone in our line of work, in the 20th century can be without. 

Any way back to the maps.

If the map as it stands is not enough to impress clients and bosses you can pick from a range of styles which you can apply simply by changing the map id. 

Here are some of my favourites

Comic style

Pirate map

Wheatsheaf

Pencil

[Break point - should be around 15-20 mins in]

But I also promised that you could use this to visualise your data. So here's a few extra lines to keep my promise.

    d3.csv("data/tubes.csv", function (error, csv) {
        csv.forEach(function (d) {
            var circle = L.circle([d.lat, d.long], d.value/8, {
                color: 'red',
                fillColor: '#f03',
                fillOpacity: 0.5
            }).addTo(mymap);
        });
        
[Download the data from coppelia and save in a subdirectory called data (you'll need to create it) next to your file]

This is just some made up data using tube stations and a random number. We could pretend it is something like passengers passing through the station.

A few years ago something half as good as this would have cost you a hefty licence fee with added costs for any additional modules. Now it's available to you through open source technology. The only cost to you is the time taken to familiarise yourself with the code (and depending how you feel about it the obligation to share in return). 

I'd like to start you off down the path of understanding of the code and building more complex maps 

Now having said earlier what a wonderful thing the open source movement is I need to now deal with one of the big drawbacks - the proliferation of tools, techniques, standards, platforms etc.

We'll find that as our mapping projects get more advanced we'll have to bring in more and more pieces from various places.

A second drawback of open source is people aren't always ready to define their terms  - you're supposed to know - somehow!

So let me help you a little by describing the parts that will go to make up your projects.

Starting with what we've got so far:

Well I've talked briefly about *javascript* the language read by browsers that makes your browser do things when you interact with it (e.g. the map zooms in as you move the mouse wheel). We are now going to look a specific open source *javascript library*, a collection of code, called *leaflet.js* (you'll see I invoke it right at the beginning of the earlier scripts)

Leaflet handles all the basic functionality of an online (or mobile map) by which I mean such things as scrolling, planning, plotting points and shapes, arranging them into layers, reading data etc. as well as some quite technical things like looking after the projection of the map.

Leaflet is brilliant at this basic stuff (and its thanks to leaflet that we were able to do so much with so little earlier on).

What it doesn't do is provide the geographical map itself.

So where do the maps come from? Well you can pick from a wide range of providers (openstreetmap, mapbox, google if you'd want anything that ugly! etc)

They will supply us with map information in the form of *tiles*. As you'd expect tiles are the things that slot into a map grid. The pictures of street, towns, rivers that fit together to make the map. As you zoom in each tile represents a smaller and smaller area and of course the pictures have to be redrawn and the tiles reloaded (if you've ever been in the middle of nowhere with you phone and tried google or apple maps you'll often see the tiles loading one by one over the terrible connection)

The people who make the tiles are themselves often dependent on other organisations who provide the data needed to make them (e.g. the street names) and these data providers are often open source collaborative enterprises providing data free of charge.

So in our earlier example mapbox provided the tiles (and the fancy styles we saw) and their tiles were built using data provided by among others openstreetmap. These tiles are pulled in from mapbox's servers when your code is opened in the browser.

So far so good.

What about the point data we overlaid on the mapbox tiles?

Hopefully most of you are familiar with json as a format for storing data. If not it looks like this.

	{
	"Title": "D.O.A.",
	"Director":  "Rudolph Maté",
	"Starring": ["Edmond O'Brien", "Pamela Britton"]
	}

It is vastly superior to tabular storage types in that it can captures nested structures like this.

	{
	"Title": "D.O.A.",
	"Director":  "Rudolph Maté",
	"Starring": [
		{
		 "Name": "Edmond O'Brien",
		 "Born": "September 10, 1915"
		},
		{
		 "Name": "Pamela Britton",
		 "Born": "March 19, 1923 "
		}
	]
	}


One such nested structure is of course geographical information. So it is no surprise that we have a data type called *geojson*. Here is an example

	{
	    "type": "Feature",
	    "properties": {
	        "name": "Ladywell Post Office",
	        "amenity": "Post Office"
	    },
	    "geometry": {
	        "type": "Point",
	        "coordinates": [-104.99404, 39.75621]
	    }
	};

And here is our example.

	Example here

Typically a geojson object will take the form of an array of features each of which have properties and a geometry. However these data structures can become quite large.

Fortunately *Mike Bostock* (famous for d3) has written something which transforms the geojson into something a lot more concise by using delta encoding (relative position) to store the co-ordinates (and it when it comes to shapes deduping shared boundaries). The resulting data structure he calls *topojson*.

Leaflet then takes these data structures and maps them onto shapes which are then overlaid on our map.

So so far we have

* *leaflet* written in *javascript*
* which pulls *tiles* from organisations like mapbox and google
* who themselves get the data from collaborative enterprises like *openstreetmap*
* and reads data in the form of *geojson* or *topojson*
* which are mapped onto shapes and overlaid onto our map

This gets us to where we are with our current example. Let's break so that people can catch up

[Break 40 mins in]

Note we have deliberately made our lives simple by limiting ourselves to points.

Things go up a notch when want to incorporate shapes!

By shapes I'm really referring to the irregular polygons which we use to describe geographical boundaries (things like country, county or postal sector boundaries). These are going to be especially useful to us when it comes to creating things like heat maps (the kind you saw in the Trussell Trust work).

For decades the standard way to represent such things was as *shape files*. These files store points, lines and polygons as vectors. They are pretty grim things to work with since the shape data is accompanied by a number of other files specifying things like the projection, the accompanying descriptive data etc. 

I try to get away from shape files as quickly as possible but since most of the useful data you'll find online comes in the form of shape files you need to know how to deal with them.

Here are some of the things I do and the tools I use

1. Load the data into R
2. Add data to shape files in R
3. Transform the shape files in R
4. Manipulate shape files using ogr2ogr

If you have R on your laptop you can run through the following example

Or without R you can add the following code to the existing file to bring in and overlay some shapes on your map

[If we need to fill time I can now do a final walk through of the code]

Some advanced topics
--------------------

[Look at this if we have time]

Journey times

Constructing alpha hulls

Connecting to google street view


Conclusion
----------














---
layout: post
title: "Putting Your First Web Map Online"
modified:
categories: tutorials
excerpt: "Getting your map up on the interwebs"
tags: [github, leaflet.js]
comments: true
author: alukach
meetup_date: 2015-03-23T18:15:00-06:00
---

{% include _toc.html %}

_Note: This tutorial is currently a work in progess._

## What are we doing?

### Goal

We're going to take some of our own data, put it on a web map, and upload it to the internet. There are a ton of ways that a person can do this. You could build your map with integrated solutions like GoogleMaps, ArcGIS Online, or Mapbox. Each of these options provide their own tiles to use as a background for your map, their own Javascript mapping library to load your map in a webpage, and their own hosting service to store your data online. But we're not going to do it that way. Instead, we are going to rely on a few different resources to put your map together and build it by hand. We're going to do it "the hard way".

### Wait, why?

> Why do it the hard way when there are a bunch of complete solutions out there that will take care of all of the details for us?

Okay, "the hard way" isn't really _that_ hard. This is a beginner-focused tutorial, after all. The problem with relying on a point-and-click solution is that a lot of the important details around how web maps work are glossed over.

We're obviously not doing this entirely by hand. We'll be "standing on the shoulders of giants", so to speak. We'll be using a third-party web mapping library, a third-party tile source, and a third-party hosting service. **The important thing to take away from this tutorial is how web mapping components fit together**. If you happen to dislike the webmapping library or tile set or hosting service, you can swap those pieces out for alternative components. This gives you freedom and control over how the tools you use for your projects and will ultimately make you a developer/cartographer/geographer/story-teller.

## How are we going to do this?

Today, we're going to focus on the following tools:

* the [Leaflet.js] web mapping library.
* tiles from [Stamen][stamen tiles].
* hosting from [Github Pages].

### Leaflet.js

[Leaflet.js] is a JavaScript web mapping library. With HTML and CSS, it is used in combination with map background imagery (web tiles) and possibly map features (points, polygons) to create interactive web maps. We touched briefly upon leaflet.js in our [February 2015 meetup](http://maptimecalgary.github.io/meetups/2015/02/08/first-meetup/). To review, take a look at [Making Your First Web Map With Leaflet!](http://lyzidiamond.com/nacis-talk/#1), [Leaflet 101 for LuxembourgJS](http://luxembourgjs.github.io/leaflet-demo/), or [An Introduction to Leaflet by MaptimeBoston](http://maptimeboston.github.io/leaflet-intro/).

### Stamen

Often, when you're putting your own data onto a web map, you'll want to put it on top of an existing background dataset (map tiles) to provide context to the data. Making your own map tiles is super interesting and fun, but is outside of the scope of this tutorial. To make this easy, we'll use some [pre-made tiles][stamen tiles] provided by [Stamen Design]. Stamen Design is a San Francisco-based research lab/development lab/design firm who have done some [amazing things](http://prettymaps.stamen.com/) with web mapping.

### Github!

When working with code, it becomes important to save revisions of your projects. This allows you to undo changes made weeks ago.  This becomes even more important when collaborating with others. To get around this, people use [revision control systems](http://en.wikipedia.org/wiki/Revision_control). One of the most popular revision control systems around today is [Git](http://en.wikipedia.org/wiki/Git_software) (fun fact: Git was created by Linus Torvalds, the creator of Linux). Going too in-depth to Git and it's inner-workings are beyond the scope of this tutorial, but it is highly encouraged that you get familiar with using Git for your projects ([check out this tutorial to learn how to use Git from the command line](https://try.github.io/levels/)).

> So, if that's Git, what is Github?

"[Github] is a web-based Git repository hosting service."<sup>[1](http://en.wikipedia.org/wiki/GitHub)</sup> Basically, Github is a service that allows you to store your Git repositories online, either publicly or privately. You can also interact with other code repositories, creating a community of online code-collaboration (enter the open-source renaissance).  For more info about Github, checkout out [Github CEO Chris Wanstrath's plenary at the 2014 ESRI Developer Summit](http://video.esri.com/watch/3223/github-ceo).

> Okay, so what does that have to do with getting my map online?

Github also offers a super-helpful feature: [Github Pages]. For any public Github repo, Github will host the content of the repo's `gh-pages` branch at `http://your_github_username.github.io/repo_name`. This allows us to host our web map for free while enjoying the version control that Git offers.

## Data

We'll be wanting to put some data onto our map. How should we format the data?

### JSON

JSON stands for JavaScript Object Notation. It allows for data to be encoded in a light-weight format that is easily understood by machines and decently readable by humans.

For example, here is some general data stored in the JSON format:

{% highlight javascript %}
{
  "glossary": {
    "title": "example glossary",
    "GlossDiv": {
      "title": "S",
      "GlossList": {
        "GlossEntry": {
          "ID": "SGML",
          "SortAs": "SGML",
          "GlossTerm": "Standard Generalized Markup Language",
          "Acronym": "SGML",
          "Abbrev": "ISO 8879:1986",
          "GlossDef": {
            "para": "A meta-markup language, used to create markup languages such as DocBook.",
            "GlossSeeAlso": ["GML", "XML"]
          },
          "GlossSee": "markup"
        }
      }
    }
  }
}
{% endhighlight %}

Here is the same data expressed as XML:


{% highlight xml %}
<!DOCTYPE glossary PUBLIC "-//OASIS//DTD DocBook V3.1//EN">
 <glossary><title>example glossary</title>
  <GlossDiv><title>S</title>
   <GlossList>
    <GlossEntry ID="SGML" SortAs="SGML">
     <GlossTerm>Standard Generalized Markup Language</GlossTerm>
     <Acronym>SGML</Acronym>
     <Abbrev>ISO 8879:1986</Abbrev>
     <GlossDef>
      <para>A meta-markup language, used to create markup
languages such as DocBook.</para>
      <GlossSeeAlso OtherTerm="GML">
      <GlossSeeAlso OtherTerm="XML">
     </GlossDef>
     <GlossSee OtherTerm="markup">
    </GlossEntry>
   </GlossList>
  </GlossDiv>
 </glossary>
{% endhighlight %}


Read more at [json.org](http://json.org/example).

### GeoJSON

From [geojson.org](http://geojson.org/geojson-spec.html):

> GeoJSON is a format for encoding a variety of geographic data structures. A GeoJSON object may represent a geometry, a feature, or a collection of features. GeoJSON supports the following geometry types: Point, LineString, Polygon, MultiPoint, MultiLineString, MultiPolygon, and GeometryCollection. Features in GeoJSON contain a geometry object and additional properties, and a feature collection represents a list of features.

Example:

{% highlight javascript %}
{
    "type": "FeatureCollection",
    "features": [
        {
            "type": "Feature",
            "geometry": {
                "type": "Point",
                "coordinates": [
                    102,
                    0.5
                ]
            },
            "properties": {
                "prop0": "value0"
            }
        },
        {
            "type": "Feature",
            "geometry": {
                "type": "LineString",
                "coordinates": [
                    [
                        102,
                        0
                    ],
                    [
                        103,
                        1
                    ],
                    [
                        104,
                        0
                    ],
                    [
                        105,
                        1
                    ]
                ]
            },
            "properties": {
                "prop0": "value0",
                "prop1": 0
            }
        },
        {
            "type": "Feature",
            "geometry": {
                "type": "Polygon",
                "coordinates": [
                    [
                        [
                            100,
                            0
                        ],
                        [
                            101,
                            0
                        ],
                        [
                            101,
                            1
                        ],
                        [
                            100,
                            1
                        ],
                        [
                            100,
                            0
                        ]
                    ]
                ]
            },
            "properties": {
                "prop0": "value0",
                "prop1": {
                    "this": "that"
                }
            }
        }
    ]
}
{% endhighlight %}

Learn more about GeoJSON from [More than you ever wanted to know about GeoJSON](http://www.macwright.org/2015/03/23/geojson-second-bite.html)

### KML

From [Google's documentation on Keyhole Markup Language](https://developers.google.com/kml/documentation/kml_tut):

> KML is a file format used to display geographic data in an Earth browser such as Google Earth, Google Maps, and Google Maps for mobile. KML uses a tag-based structure with nested elements and attributes and is based on the XML standard.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<kml xmlns="http://www.opengis.net/kml/2.2">
  <Document>
    <Placemark>
      <name>CDATA example</name>
      <description>
        <![CDATA[
          <h1>CDATA Tags are useful!</h1>
          <p><font color="red">Text is <i>more readable</i> and
          <b>easier to write</b> when you can avoid using entity
          references.</font></p>
        ]]>
      </description>
      <Point>
        <coordinates>102.595626,14.996729</coordinates>
      </Point>
    </Placemark>
  </Document>
</kml>
{% endhighlight %}

### TopoJSON

From [Mike Bostock's Topojson repo](https://github.com/mbostock/topojson):

> TopoJSON is an extension of GeoJSON that encodes topology. Rather than representing geometries discretely, geometries in TopoJSON files are stitched together from shared line segments called arcs. TopoJSON eliminates redundancy, offering much more compact representations of geometry than with GeoJSON; typical TopoJSON files are 80% smaller than their GeoJSON equivalents. In addition, TopoJSON facilitates applications that use topology, such as topology-preserving shape simplification, automatic map coloring, and cartograms.

Example of [U.S. Counties TopoJSON](http://bl.ocks.org/mbostock/4122298):

<iframe src="http://bl.ocks.org/mbostock/raw/4122298/" width="100%"></iframe>

See more in the [TopoJSON Gallery](https://github.com/mbostock/topojson/wiki/Gallery).

[leaflet.js]: http://leafletjs.com/
[stamen design]: http://stamen.com
[stamen tiles]: http://maps.stamen.com/
[github]: http://github.com
[github pages]: https://pages.github.com/

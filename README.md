# MaptimeCalgary Website

Hey there! Contributions to the MaptimeCalgary website are welcomed and encouraged! Feel free to improve upon any aspect of the site. This site is a [Jekyll](http://jekyllrb.com/) site, hosted on [Github Pages](https://pages.github.com/).

We are using the [Minimal Mistakes](http://mmistakes.github.io/minimal-mistakes) theme. To learn how to install and use this theme check out the [Setup Guide](http://mmistakes.github.io/minimal-mistakes/theme-setup/) for more information. If you're looking to add a blog post or tutorial, take a look at the [sample post](http://mmistakes.github.io/minimal-mistakes/sample-post/) and [its source code](https://raw.githubusercontent.com/mmistakes/minimal-mistakes/master/_posts/2011-03-10-sample-post.md) to get an idea of how to format your content.

We've modified the theme to allow for [Leaflet](http://leafletjs.com/) maps to be used a post/page's [hero image](http://en.wikipedia.org/wiki/Hero_image).  If you're looking to add a map to a post or a page, you can do so by add something similar to the following to the following:

``` yaml
...  # Other metadata
map:
  mapboxlayer: examples.map-i86nkdio
  attribution: Map data &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors, <a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="http://mapbox.com">Mapbox</a>
  options:  # Pass through any supported map option: http://leafletjs.com/reference.html#map-options
    center: "[51.04, -114.075]"
    zoom: 10
    attributionControl: false
  markers:
    - location: [51.04, -114.075]
      content: I'm open!
      open: true
    - location: [51.09, -114.07]
      content: Closed popup.
---

```
## To Contribute

1. Fork the repo.
1. Make a change.
1. Open a pull request.

Learn more about contributing [here](https://guides.github.com/activities/forking/).


## To Join

To join MaptimeCalgary, all you need to do is show up to the next meetup. That's it, you're in!

To join the MaptimeCalgary Organization on Github, open up a [Github Issue](https://github.com/MaptimeCalgary/maptimecalgary.github.io/issues/new) with a brief introduction.

For any other questions or comments, feel free to drop us a line at `maptimecalgary@gmail.com`.

# Mapbox 🗺️
In this lesson, we're going to explore how to add interactive maps to your Ruby on Rails application using Mapbox, a powerful mapping and location service. Adding maps can enhance the user experience significantly, allowing you to display location-based data, create route navigations, show geographical data, and much more. By the end of this lesson, you will know how to embed a Mapbox map into your Rails application, customize it to display multiple locations, and interact with it using JavaScript.

## Getting Started
First, you need to have a [Mapbox](https://www.mapbox.com/) account. If you don't have one, go to Mapbox's website and sign up. After signing up, you will have access to your Mapbox access token, which is essential for using their services in your application.

## Set Up Your Rails Application
Before integrating Mapbox, ensure your Ruby on Rails application is set up and running. For demonstration purposes, let's assume you're building a location-based application where users can discover interesting places in a city.

Create a new Rails application if you haven't already:

```bash
rails new mapbox_demo
cd mapbox_demo
```

And ensure you have a model to store your location data. For example, you might have a `Place` model with `name`, `latitude`, and `longitude` attributes.

## Include Mapbox in Your Application
To use Mapbox, you need to include Mapbox's JavaScript and CSS files in your application. Add the following lines to your `app/assets/stylesheets/application.css` and `app/javascript/packs/application.js` files, respectively:

<!-- or add to <head> or importmap -->
```css
/* app/assets/stylesheets/application.css */
@import url('https://api.mapbox.com/mapbox-gl-js/v2.3.1/mapbox-gl.css');
```

```javascript
// app/javascript/packs/application.js
import mapboxgl from 'https://api.mapbox.com/mapbox-gl-js/v2.3.1/mapbox-gl.js';

mapboxgl.accessToken = 'YOUR_MAPBOX_ACCESS_TOKEN';
```

Replace 'YOUR_MAPBOX_ACCESS_TOKEN' with your actual Mapbox access token. Remember to only paste your access token into your code in development and to use environment variables before commiting or pushing code to GitHub.

## Embed a Mapbox Map into a View
Now, let's add a Mapbox map to one of your views. For example, in app/views/places/index.html.erb, you could add:

```html
<div id='map' style='width: 400px; height: 300px;'></div>

<script>
  mapboxgl.accessToken = 'YOUR_MAPBOX_ACCESS_TOKEN';
  var map = new mapboxgl.Map({
    container: 'map',
    style: 'mapbox://styles/mapbox/streets-v11',
    center: [-74.0066, 40.7135], // starting position
    zoom: 9 // starting zoom
  });
</script>
```

Make sure to replace 'YOUR_MAPBOX_ACCESS_TOKEN' with your actual Mapbox access token.

## Display Dynamic Locations on the Map
Assuming you have a list of places in your PlacesController that you want to display on the map, you can dynamically add markers to the map using Rails' ERB syntax and JavaScript. Modify your view to loop through your places and add markers:

```erb
<div id='map' style='width: 400px; height: 300px;'></div>

<script>
  mapboxgl.accessToken = 'YOUR_MAPBOX_ACCESS_TOKEN';
  var map = new mapboxgl.Map({
    container: 'map',
    style: 'mapbox://styles/mapbox/streets-v11',
    center: [-74.0066, 40.7135],
    zoom: 9
  });

  <% @places.each do |place| %>
    new mapboxgl.Marker()
      .setLngLat([<%= place.longitude %>, <%= place.latitude %>])
      .setPopup(new mapboxgl.Popup({ offset: 25 })
      .setHTML('<h3><%= place.name %></h3>'))
      .addTo(map);
  <% end %>
</script>
```

This script iterates over each place, creating a new marker for each location and adding it to the map.

## Enhance the Map
Mapbox provides a wide range of customization options. You can change the map's style, add navigation controls, create custom markers, and much more. For example, to add zoom and rotation controls to the map, you can add the following line to your JavaScript:

```javascript
map.addControl(new mapboxgl.NavigationControl());
```


## Conclusion
You've now successfully integrated a Mapbox map into your Ruby on Rails application and customized it

## Resources
- Explore the [Mapbox GL JS documentation](https://docs.mapbox.com/mapbox-gl-js/guides) to discover all the ways you can customize your map.
- Consider exploring the [geocoder gem](https://github.com/alexreisner/geocoder) for integrating location-based features like: geocoding, reverse geocoding, calculating distance, filtering within a range, etc.

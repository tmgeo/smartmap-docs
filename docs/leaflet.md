# Leaflet

## Introduction

Leaflet is the leading open-source JavaScript library for mobile-friendly interactive maps. Weighing just about 38 KB of JS, it has all the mapping features most developers ever need. Leaflet is designed with simplicity, performance and usability in mind.

This is a tutorial on how to use Leaflet library to consume TM Smartmap API in your own custom application.

## Dependencies

Before writing any code for the map, you need to do the following preparation steps on your page:

- Include Leaflet CSS file in the head section of your document:

```html
<link
  rel="stylesheet"
  href="https://unpkg.com/leaflet@1.4.0/dist/leaflet.css"
  integrity="sha512-puBpdR0798OZvTTbP4A8Ix/l+A4dHDD0DGqYW6RQ+9jxkRFclaxxQb/SJAWZfWAkuyeQUytO7+7N4QKrDh+drA=="
  crossorigin=""
/>
```

- Include Leaflet JavaScript file after Leaflet’s CSS::

```html
<!-- Make sure you put this AFTER Leaflet's CSS -->
<script
  src="https://unpkg.com/leaflet@1.4.0/dist/leaflet.js"
  integrity="sha512-QVftwZFqvtRNi0ZyCtsznlKSWOStnDORoefr1enyq5mVL4tmKB3S/EnC3rRJcxCPavG10IcrVGSmPh6Qw5lwrg=="
  crossorigin=""
></script>
```

- Put a div element with a certain id where you want your map to be:

```html
<div id="mapid"></div>
```

- Make sure the map container has a defined height, for example by setting it in CSS:

```css
#mapid {
  height: 180px;
}
```

Now you’re ready to initialize the map and do some
stuff with it.

## The Basemap

[Basic Example](_examples/leaflet/basic.html ':include :type=iframe width=500px height=400px')

Let’s create a map of the center of London with pretty Mapbox Streets tiles. First we’ll initialize the map and set its view to our chosen geographical coordinates and a zoom level:

```javascript
var myMap = L.map('mapid', {
  zoom: 6,
  center: [3.747128, 108.68593],
  attributionControl: true,
});
```

By default (as we didn’t pass any options when creating the map instance), all mouse and touch interactions on the map are enabled, and it has zoom and attribution controls.

Note that setView call also returns the map object — most Leaflet methods act like this when they don’t return an explicit value, which allows convenient jQuery-like method chaining.

Next we’ll add a tile layer to add to our map, in this case it’s a Mapbox Streets tile layer. Creating a tile layer usually involves setting the URL template for the tile images, the attribution text and the maximum zoom level of the layer. In this example we’ll use the mapbox.streets tiles from Mapbox’s “Classic maps” (in order to use tiles from Mapbox, you must also request an access token).

```javascript
var tmLayer = L.tileLayer
  .wms(
    'http://lbd-api.tk/api/map/wms?workspace={serverWorkspace}&api_key={accessToken}',
    {
      serverWorkspace: 'Malaysia_WMS',
      accessToken: { YOUR_TOKEN_HERE },
      layers: 'Malaysia:TMSmartmap',
      format: 'image/png',
      crs: L.CRS.EPSG4326,
      tiled: true,
      transparent: true,
      attribution: 'TM SmartMap © 2019 Telekom Malaysia',
    }
  )
  .addTo(myMap);
```

Make sure all the code is called after the div and leaflet.js inclusion. That’s it! You have a working Leaflet map now.

It’s worth noting that Leaflet is provider-agnostic, meaning that it doesn’t enforce a particular choice of providers for tiles. You can try replacing mapbox.streets with mapbox.satellite, and see what happens. Also, Leaflet doesn’t even contain a single provider-specific line of code, so you’re free to use other providers if you need to (we’d suggest Mapbox though, it looks beautiful).

Whenever using anything based on OpenStreetMap, an attribution is obligatory as per the copyright notice. Most other tile providers (such as MapBox, Stamen or Thunderforest) require an attribution as well. Make sure to give credit where credit is due.

## Markers

Besides tile layers, you can easily add other things to your map, including markers, polylines, polygons, circles, and popups. Let’s add a marker:

<iframe width="100%" height="650" src="_examples/leaflet/withMarker.html" /></iframe>

Besides tile layers, you can easily add other things to your map, including markers, polylines, polygons, circles, and popups. Let’s add a marker:

```javascript
var marker = L.marker([51.5, -0.09]).addTo(mymap);
```

Adding a circle is the same (except for specifying the radius in meters as a second argument), but lets you control how it looks by passing options as the last argument when creating the object:

```javascript
var circle = L.circle([51.508, -0.11], {
  color: 'red',
  fillColor: '#f03',
  fillOpacity: 0.5,
  radius: 500,
}).addTo(mymap);
```

Adding a polygon is as easy:

```javascript
var polygon = L.polygon([
  [51.509, -0.08],
  [51.503, -0.06],
  [51.51, -0.047],
]).addTo(mymap);
```

## Search

<iframe width="100%" height="650" src="_examples/leaflet/withSearch.html" /></iframe>

To search location and display marker on map.

Create Text Search with Button in body

```html
<label for="inp" class="inp">
  <input
    type="text"
    id="inp"
    placeholder="&nbsp;"
    onkeypress="showMarkers(event, 'search')"
  />
  <span class="label">Search</span>
  <button class="btn" disabled id="but" onclick="showMarkers(event, 'search')">
    <i class="fas fa-search icon"></i>
  </button>
  <button class="btn" onclick="removeMarkers('remove')">
    <i class="fas fa-times icon"></i>
  </button>
</label>
```

Now add function showMarkers() in script

```javascript
function showMarkers(e, type) {
  var value = document.getElementById('inp').value;
  if (value !== '') {
    document.getElementById('but').disabled = false;
  } else {
    document.getElementById('but').disabled = true;
  }
  if (e.keyCode === 13 || e.keyCode === undefined) {
    removeMarkers(type);
    fetch(`http://lbd-api.tk/api/poi?api_key={YOUR_TOKEN_HERE}&q=${value}`)
      .then(res => res.json())
      .then(data => {
        data.map(data => {
          arrayMarkers.push(
            L.marker([data.location.lat, data.location.lon])
              .addTo(myMap)
              .bindPopup(`${data.formatted_address}`)
              .openPopup()
          );
        });
      });
  }
}
```

And add function removeMarkers() in script

```javascript
var arrayMarkers = [];
function removeMarkers(type) {
  if (arrayMarkers.length > 0) {
    arrayMarkers.map(marker => {
      myMap.removeLayer(marker);
    });
    if (type !== 'search') document.getElementById('inp').value = '';
    document.getElementById('but').disabled = true;
  }
}
```

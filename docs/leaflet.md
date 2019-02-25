# Leaflet

Leaflet is ...

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

<iframe width="100%" height="650" src="_examples/leaflet/withSearch.html" /></iframe>

Besides tile layers, you can easily add other things to your map, including markers, polylines, polygons, circles, and popups. Let’s add a marker:

var marker = L.marker([51.5, -0.09]).addTo(mymap);
Adding a circle is the same (except for specifying the radius in meters as a second argument), but lets you control how it looks by passing options as the last argument when creating the object:

```javascript
var circle = L.circle([51.508, -0.11], {
    color: 'red',
    fillColor: '#f03',
    fillOpacity: 0.5,
    radius: 500
}).addTo(mymap);
Adding a polygon is as easy:

var polygon = L.polygon([
    [51.509, -0.08],
    [51.503, -0.06],
    [51.51, -0.047]
]).addTo(mymap);
```

## Search

<iframe width="100%" height="650" src="_examples/leaflet/withMarker.html" /></iframe>

Aliqua quis dolore ipsum amet. Cupidatat reprehenderit excepteur aliqua excepteur qui consectetur deserunt deserunt reprehenderit duis laboris ut officia anim. Minim sunt officia mollit est. Amet occaecat aliquip dolore aliquip ipsum pariatur laboris nulla ea exercitation esse ut fugiat. Aute cupidatat reprehenderit irure qui officia eiusmod non sit ad excepteur sunt consectetur irure sunt. Proident velit tempor ipsum pariatur dolore proident velit mollit quis incididunt laboris. Nostrud dolore esse ut ipsum consequat sunt in mollit.

Et mollit ex culpa mollit voluptate incididunt irure aliqua aliqua exercitation dolore proident. Reprehenderit duis pariatur quis non et aute sint deserunt. Lorem et magna mollit nulla adipisicing anim consequat reprehenderit aute minim. Officia amet magna do in aliquip. Do aliqua commodo ipsum quis.

Irure sint aute culpa nostrud amet quis tempor magna fugiat magna. Ut sint aliquip sit consequat sit veniam ut aliquip qui ullamco cillum in. Aliquip incididunt aliquip irure exercitation magna. In eu minim amet tempor esse dolor ea. Culpa laborum irure aute exercitation ipsum excepteur.

Qui laborum sunt ipsum aliquip esse irure nisi laborum velit. Eu non do commodo qui ex aute occaecat incididunt anim. Consectetur ipsum Lorem ad ea eiusmod. Est anim consequat Lorem incididunt dolore nulla. Est consectetur eiusmod sint adipisicing velit cupidatat minim enim consequat esse tempor reprehenderit.

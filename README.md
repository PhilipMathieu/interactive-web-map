# Interactive Web Map with PMTiles

## 1. Prepare Dataset

[Instructions for writing GeoJson files]

## 2. Convert to PMTiles Format

Download and install `tippecanoe` following [the instructions in the GitHub repository](https://github.com/felt/tippecanoe#installation).

Note: `tippecanoe` is only compatible with Unix-based systems. If you are using Windows, you can use the Windows Subsystem for Linux (WSL) to run `tippecanoe`. You can also run install and run the command in a Docker container such as [`osgeo/gdal`](https://github.com/OSGeo/gdal/pkgs/container/gdal).

To convert the file, run the following command:

```bash
tippecanoe -zg --output=docs/tiles/ski_areas_all.pmtiles --projection=EPSG:4326 data/example/ski_areas_all.geojson
```

`tippecanoe` is an extremely flexible tool with many options to adjust how the tiles are generated. For more information, see the [tippecanoe documentation](https://github.com/felt/tippecanoe?tab=readme-ov-file#try-this-first).

## 3. Add to Map

The map code is included at the bottom of [`docs/index.html`](docs/index.html). The following code is added to the map:

```javascript
// Setup PMTiles protocol with your map instance
let protocol = new pmtiles.Protocol();
maplibregl.addProtocol("pmtiles", protocol.tile);

// Create a new map instance
const map = new maplibregl.Map({
    container: 'map', // container ID
    style: 'https://api.maptiler.com/maps/satellite/style.json?key=MAPTILER_API_KEY', // style URL, replace with your own style if necessary
    center: [0, 0], // starting position [lng, lat]
    zoom: 1 // starting zoom
});


// Example of adding a PMTiles source and layer to your map
map.on('load', function () {
    map.addSource('skiAreas', {
        'type': 'vector',
        'url': 'pmtiles://YOUR_PROJECT_URL/tiles/ski_areas_all.pmtiles'
    });
    map.addLayer({
        'id': 'skiAreasCircles',
        'type': 'circle',
        'source': 'skiAreas',
        'source-layer': 'ski_areas_all',
        'paint': {
            'circle-radius': 6,
            'circle-color': '#B42222'
        }
    });
});
```
# Interactive Web Map with Bootstrap, MapLibre GL JS, PMTiles, and GitHub Pages

This project demonstrates how to create an interactive web map for sharing geospatial projects. The web map consists of a single HTML file that uses Bootstrap to simplify styling. The map itself is rendered by MapLibre GL JS. Data is stored in the PMTiles format, a single-file format that simple to use and easy to deploy, making it a great choice for small to medium-sized datasets. Together, these packages make it possible to serve a fully-featured web map with static hosting services such as GitHub Pages.

This project was created by Philip Mathieu. If you like my work, consider [buying me a coffee](https://www.buymeacoffee.com/philipmathieu)!

[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/philipmathieu)

## 1. Prepare Dataset

This project assumes that you are working with a GeoJSON data source. If you are working with a different format, you will need to convert it to GeoJSON with a tool such as [GDAL](https://gdal.org/index.html) before proceeding. An example dataset is included in the `data/example` folder.

In the case that you are coming from a `geopandas` project, you can convert the `GeoDataFrame` to GeoJSON with the following code:

```python
import geopandas as gpd

gdf = gpd.GeoDataFrame()
# ... add data to gdf ...
gdf.to_file("path/to/file.geojson", driver="GeoJSON")
```

For more information on geopandas, see the [geopandas documentation](https://geopandas.org/).

## 2. Convert to PMTiles Format

Download and install `tippecanoe` (v2.17 or later) following [the instructions in the GitHub repository](https://github.com/felt/tippecanoe#installation).

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
        'url': 'pmtiles://tiles/ski_areas_all.pmtiles'
    });
    map.addLayer({
        'id': 'skiAreasCircles',
        'type': 'circle',
        'source': 'skiAreas',
        'source-layer': 'ski_areas_all',
        'paint': {
            'circle-radius': 6,
            'circle-color': 'rgba(255,255,255,0.5)'
        }
    });
});
```

## 4. Deploy with GitHub Pages

To deploy the map, push the changes to the `main` branch and enable GitHub Pages in the repository settings (https://github.com/YOUR_GITHUB_UERNAME/YOUR_PROJECT_NAME/settings/pages).

1. Under "Build and Deployment" choose "Deploy from a branch"
2. Select the `main` branch
3. Select the `/docs` folder
4. Click "Save"

The map will be available at [https://YOUR_GITHUB_USERNAME.github.io/YOUR_PROJECT_NAME/](https://YOUR_GITHUB_USERNAME.github.io/YOUR_PROJECT_NAME/).

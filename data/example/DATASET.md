# Sample Data
This project uses a geojson file containing locations of US ski areas. The original shapefile was retrived from:

https://www.nohrsc.noaa.gov/gisdatasets/

The original shapefile was converted to geojson using the following command:

```bash
ogr2ogr -f GeoJSON ski_areas.geojson ski_areas.shp
```

And to PMTiles with the following command:

```bash
tippecanoe -zg --output=docs/tiles/ski_areas_all.pmtiles --projection=EPSG:4326 data/example/ski_areas_all.geojson
```
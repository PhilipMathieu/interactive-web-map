# Sample Data
This project uses a geojson file containing locations of US ski areas. The original shapefile was retrived from:

https://www.nohrsc.noaa.gov/gisdatasets/

The original shapefile was converted to geojson using the following command:

```bash
ogr2ogr -f GeoJSON ski_areas.geojson ski_areas.shp
```

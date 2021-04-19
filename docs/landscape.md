# Urban landscape analysis
## Isovist and isovist field
### Indices for star-shaped polygons
## Sky Map
```python
from geopandas import GeoDataFrame
from shapely.geometry import Point
from t4gpd.demos.GeoDataFrameDemos import GeoDataFrameDemos
from t4gpd.morph.geoProcesses.SkyMap2D import SkyMap2D
from t4gpd.morph.geoProcesses.STGeoProcess import STGeoProcess

buildings = GeoDataFrameDemos.districtRoyaleInNantesBuildings()

pts = [ Point((355154.0, 6689292.4)), Point((355206.0, 6689346.6)),
	Point((355229.6, 6689294.1)), Point((355190.5, 6689306.1)),
	Point((355143.0, 6689359.4)) ]
viewpoints = GeoDataFrame([{'geometry': p} for p in pts], crs=buildings.crs)

op = SkyMap2D(buildings, nRays=180, size=8.0, maxRayLen=100.0, 
	elevationFieldname='HAUTEUR', projectionName='Stereographic')
skymaps = STGeoProcess(op, viewpoints).run()
```

To map it via matplotlib, proceed as follows:

```python
import matplotlib.pyplot as plt
from shapely.geometry import box

minx, miny, maxx, maxy = box(*skymaps.total_bounds).buffer(10).bounds

_, basemap = plt.subplots(figsize=(0.75*8.26, 0.75*8.26))
buildings.plot(ax=basemap, color='lightgrey', edgecolor='white', linewidth=0.5)
skymaps.plot(ax=basemap, color='dimgrey')
viewpoints.plot(ax=basemap, color='black', marker='+')
plt.axis('off')
plt.axis([minx, maxx, miny, maxy])
plt.savefig('img/skymaps.png')
```

![Sky Maps](img/skymaps.png)

## Sky and Sun Maps

```python
from datetime import date, time
from geopandas import GeoDataFrame
from shapely.geometry import Point
from t4gpd.demos.GeoDataFrameDemos import GeoDataFrameDemos
from t4gpd.morph.geoProcesses.SkyMap2D import SkyMap2D
from t4gpd.morph.geoProcesses.STGeoProcess import STGeoProcess
from t4gpd.sun.STSunMap2D import STSunMap2D

buildings = GeoDataFrameDemos.districtRoyaleInNantesBuildings()
viewpoint = GeoDataFrame([{'geometry': Point((355143.0, 6689359.4))}],
	crs=buildings.crs)

op = SkyMap2D(buildings, nRays=180, size=7.0, maxRayLen=100.0, 
	elevationFieldname='HAUTEUR', projectionName='Stereographic')
skymap = STGeoProcess(op, viewpoint).run()

datetimes = [ date(2020, month, 21) for month in (3, 6, 12)]
datetimes += [ time(hour) for hour in range(6, 19, 2) ]

sunmap = STSunMap2D(viewpoint, datetimes, size=7.0,
	projectionName='Stereographic').run()
```

To map it via matplotlib, proceed as follows:

```python
import matplotlib.pyplot as plt
from shapely.geometry import box

minx, miny, maxx, maxy = box(*skymap.total_bounds).buffer(10).bounds

_, basemap = plt.subplots(figsize=(0.5*8.26, 0.5*8.26))
buildings.plot(ax=basemap, color='dimgrey', edgecolor='white', linewidth=0.5)

skymap.plot(ax=basemap, color='lightgrey')
viewpoint.plot(ax=basemap, color='black', marker='+')
sunmap[ sunmap.label == 'framework' ].plot(ax=basemap, linewidth=0.5, color='dimgrey')
sunmap[ sunmap.label.str.startswith('2020-') ].plot(ax=basemap, linewidth=0.5, color='red')
sunmap[ sunmap.label.str.endswith(':00:00') ].plot(ax=basemap, linewidth=0.5, color='blue')
plt.axis('off')
plt.axis([minx, maxx, miny, maxy])
plt.savefig('img/skySunMaps.png')
```

![Sky Maps](img/skySunMaps.png)

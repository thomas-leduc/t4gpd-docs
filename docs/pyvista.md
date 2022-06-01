# Interfacing with PyVista and VTK 3D plotting (version 0.4.0+)

As written on [docs.pyvista.org](https://docs.pyvista.org/), PyVista is "a 
high-level API to the [3D] Visualization Toolkit ([VTK](https://vtk.org/))". Provided you have installed it correctly (which conda does very well via a ```conda install pyvista```!), the following instructions allow you to switch from the traditional 2D geomatics environment to a 3D environment:

```python
from numpy.random import randint
from t4gpd.commons.GeomLib import GeomLib
from t4gpd.demos.GeoDataFrameDemos import GeoDataFrameDemos
from t4gpd.morph.geoProcesses.FootprintExtruder import FootprintExtruder
from t4gpd.morph.geoProcesses.STGeoProcess import STGeoProcess
from t4gpd.morph.STPointsDensifier2 import STPointsDensifier2
from t4gpd.pyvista.ToUnstructuredGrid import ToUnstructuredGrid

buildings = GeoDataFrameDemos.regularGridOfPlots(3, 4, dw=5.0)
buildings['n_floors'] = randint(2, 7, size=len(buildings))
buildings['height'] = 3.0 * buildings.n_floors

sensors = STPointsDensifier2(buildings, curvAbsc=[0.25, 0.75], pathidFieldname=None).run()
sensors['height2'] = sensors.height.apply(lambda h: randint(1.0, h - 1.0))
sensors['__TMP__'] = list(zip(sensors.geometry, sensors.height2))
sensors.geometry = sensors.__TMP__.apply(lambda t: GeomLib.forceZCoordinateToZ0(t[0], z0=t[1]))
sensors.drop(columns=['__TMP__'], inplace=True)

op = FootprintExtruder(buildings, 'height', forceZCoordToZero=True)
buildingsIn3d = STGeoProcess(op, buildings).run()

scene1 = ToUnstructuredGrid([buildingsIn3d, sensors], 'height2').run()
scene1.plot(scalars='height2', cmap='gist_earth', show_edges=False,
	show_scalar_bar=True, point_size=20.0, render_points_as_spheres=True,
	screenshot='pyvista_1.png')
```

As you may notice, the *t4gpd.pyvista.ToUnstructuredGrid* class provides a way to wrap a list of *GeoDataFrame*s before passing them to PyVista.

![PyQGIS](img/pyvista_1.png)

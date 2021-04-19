# Reading and writing files

## Reading files
### Reading CIR (Solene) files

[Solene](https://aau.archi.fr/crenau/solene/) is an (old) software tool developed in the 1990s and early 2000s at the AAU-CERMA laboratory (the forerunner of the current [AAU-CRENAU laboratory](https://aau.archi.fr/)). Originally dedicated to the urban and architectural projects' solar and light access and thermal radiation calculations, it was then extended to the study of different dimensions of the urban microclimate. This project is now archived. Its sources are distributed under the GNU General Public License at [SourceSup](https://sourcesup.renater.fr/projects/solenetb/).

To load a geometry file stored in the Solene file format (.cir extension), proceed as follows:

```python
from t4gpd.io.CirReader import CirReader

myGdf = CirReader('../data/cube_unitaire.cir').run()
```

The resulting object named *myGdf* is an instance of GeoDataFrame.

### Partial support of CityGML files

The [FZK-Haus](https://www.citygmlwiki.org/index.php?title=FZK_Haus) is a data set provided by the Karlsruhe Institute of Technology (KIT), Institute for Applied Computer Science (Campus North). Let's load it using *t4gpd.io.CityGMLReader*:

```python
from t4gpd.io.CityGMLReader import CityGMLReader

myGdf = CityGMLReader('./AC11-FZK-Haus_LoD4_SpaceSolid_v1.0.0.gml',
	srcEpsgCode='EPSG:3068').run()
```

The resulting object named *myGdf* is an instance of GeoDataFrame. To map it via matplotlib, proceed as follows:

```python
import matplotlib.pyplot as plt

_, basemap = plt.subplots(figsize=(0.25*8.26, 0.25*8.26))
myGdf.boundary.plot(ax=basemap, color='grey', linewidth=0.15)
plt.axis('off')
plt.savefig('img/citygml.png')
```

![CityGML](img/citygml.png)

### Reading OBJ files

To load a geometry file stored in the OBJ file format, proceed as follows:

```python
from t4gpd.io.ObjReader import ObjReader

myGdf = ObjReader('/tmp/buildings.obj').run()
```

The resulting object named *myGdf* is an instance of GeoDataFrame.

### Reading MSH (GMSH) files

To load a geometry file stored in the GMSH/MSH file format (.msh extension), proceed as follows:

```python
from t4gpd.demos.GeoDataFrameDemos import GeoDataFrameDemos
from t4gpd.io.MshReader import MshReader

buildings = GeoDataFrameDemos.districtRoyaleInNantesBuildings()

myGdf = MshReader('/tmp/buildings.msh', bbox=buildings.total_bounds, crs='EPSG:2154').run()
```

The resulting object named *myGdf* is an instance of GeoDataFrame.

## Writing files

### Preamble: How to extrude 2D geometry to produce a closed volume

```python
from t4gpd.demos.GeoDataFrameDemos import GeoDataFrameDemos
from t4gpd.morph.geoProcesses.FootprintExtruder import FootprintExtruder
from t4gpd.morph.geoProcesses.STGeoProcess import STGeoProcess

building = GeoDataFrameDemos.singleBuildingInNantes()

op = FootprintExtruder(building, elevationFieldname='HAUTEUR', forceZCoordToZero=True)
buildingVolume = STGeoProcess(op, building).run()
```

![2d3d](img/geom_2d3d.png)

### Writing CIR (Solene) files

[Solene](https://aau.archi.fr/crenau/solene/) is an (old) software tool developed in the 1990s and early 2000s at the AAU-CERMA laboratory (the forerunner of the current [AAU-CRENAU laboratory](https://aau.archi.fr/)). Originally dedicated to the urban and architectural projects' solar and light access and thermal radiation calculations, it was then extended to the study of different dimensions of the urban microclimate. This project is now archived. Its sources are distributed under the GNU General Public License at [SourceSup](https://sourcesup.renater.fr/projects/solenetb/).

To save a GeoDataFrame in the Solene file format (.cir extension), proceed as follows:

```python
from t4gpd.demos.GeoDataFrameDemos import GeoDataFrameDemos
from t4gpd.io.CirWriter import CirWriter

buildings = GeoDataFrameDemos.districtRoyaleInNantesBuildings()

CirWriter(buildings, '/tmp/buildings.cir').run()
```

### Writing GEO (GMSH) files

```python
from t4gpd.demos.GeoDataFrameDemos import GeoDataFrameDemos
from t4gpd.io.GeoWriter import GeoWriter

buildings = GeoDataFrameDemos.districtRoyaleInNantesBuildings()

GeoWriter(buildings, '/tmp/buildings.geo', characteristicLength=10.0, toLocalCrs=True).run()
```

It is then possible, as shown in the figure below, to open the file '/tmp/buildings.geo' in the [GMSH](http://gmsh.info/) three-dimensional finite element mesh generator, then to mesh it.

![Gmsh](img/gmsh.png)

### Writing OBJ files

```python
from t4gpd.demos.GeoDataFrameDemos import GeoDataFrameDemos
from t4gpd.io.ObjWriter import ObjWriter

buildings = GeoDataFrameDemos.districtRoyaleInNantesBuildings()
buildings = buildings.explode()

ObjWriter(buildings, '/tmp/buildings.obj').run()
```

It is then possible, as an example, to open the file '/tmp/buildings.obj' in [Meshlab](https://www.meshlab.net/) or [ParaView](https://www.paraview.org/).

![Meshlab](img/meshlab.png)

### Writing SVG files

```python
from t4gpd.demos.GeoDataFrameDemos import GeoDataFrameDemos
from t4gpd.io.SVGWriter import SVGWriter

buildings = GeoDataFrameDemos.districtRoyaleInNantesBuildings()

SVGWriter(buildings, '/tmp/buildings.svg', bbox=None, color='black').run()
```

It is then possible, as an example, to open the file '/tmp/buildings.svg' in [Inkscape](https://inkscape.org/).

![Inkscape](img/inkscape.png)

### Writing VTU files

```python
from t4gpd.demos.GeoDataFrameDemos import GeoDataFrameDemos
from t4gpd.io.VTUWriter import VTUWriter

building = GeoDataFrameDemos.singleBuildingInNantes()

VTUWriter(building, '/tmp/building.vtu').run()
```

It is then possible to open the file '/tmp/building.vtu' in [ParaView](https://www.paraview.org/).

![ParaView](img/paraview.png)
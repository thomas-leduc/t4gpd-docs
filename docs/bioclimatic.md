# Urban bioclimatic shape analysis
## Sky View Factor
## Ground shadows

<!--
## Comfort Indexes
### Mean radiant temperature

```python
from t4gpd.comfort.MeanRadiantTemperature import MeanRadiantTemperature
from t4gpd.morph.geoProcesses.STGeoProcess import STGeoProcess

op = MeanRadiantTemperature(measuresGdf)
mrtGdf = STGeoProcess(op, measuresGdf).run()
```

### Empirical Thermal Indexes

```python
from t4gpd.comfort.EmpiricalThermalIndexes import EmpiricalThermalIndexes
from t4gpd.morph.geoProcesses.STGeoProcess import STGeoProcess

measuresGdf['TC_mean'] = (measuresGdf['Temp_C_Avg(1)'] + measuresGdf['Temp_C_Avg(2)']) / 2.0

op = EmpiricalThermalIndexes(measuresGdf, AirTC='TC_mean')
etiGdf = STGeoProcess(op, measuresGdf).run()
```

### Linear Thermal Indexes

```python
from t4gpd.comfort.LinearThermalIndexes import LinearThermalIndexes
from t4gpd.morph.geoProcesses.STGeoProcess import STGeoProcess

measuresGdf['TC_mean'] = (measuresGdf['Temp_C_Avg(1)'] + measuresGdf['Temp_C_Avg(2)']) / 2.0

op = LinearThermalIndexes(measuresGdf, AirTC='TC_mean')
ltiGdf = STGeoProcess(op, measuresGdf).run()
```

### Universal Thermal Indexes

```python
from t4gpd.comfort.MeanRadiantTemperature import MeanRadiantTemperature
from t4gpd.comfort.UniversalThermalIndexes import UniversalThermalIndexes
from t4gpd.morph.geoProcesses.STGeoProcess import STGeoProcess

measuresGdf['TC_mean'] = (measuresGdf['Temp_C_Avg(1)'] + measuresGdf['Temp_C_Avg(2)']) / 2.0

op1 = MeanRadiantTemperature(measuresGdf)
mrtGdf = STGeoProcess(op1, measuresGdf).run()

op2 = UniversalThermalIndexes(mrtGdf, AirTC='TC_mean')
mrtUtiGdf = STGeoProcess(op2, mrtGdf).run()
```
-->
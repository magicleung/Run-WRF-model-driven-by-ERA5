# Run-WRF-model-driven-by-ERA5
_**This guide takes you through the process of running the Weather Research and Forecasting (WRF) Model using the ERA5 reanalysis data.**_

## 1. Required Fields
  * #### 3D Data (e.g. data on pressure levels)：
        'geopotential', 'relative_humidity', 'specific_humidity', 'temperature', 'u_component_of_wind', 'v_component_of_wind'
  * #### 2D Data：
        'surface_pressure', 'mean_sea_level_pressure', '10m_u_component_of_wind', '10m_v_component_of_wind', '2m_temperature', 'sea_surface_temperature', 'skin_temperature', '2m_dewpoint_temperature', 'snow_depth', 'sea_ice_cover', 'land_sea_mask', 'soil_type', 'soil_temperature_level_1', 'soil_temperature_level_2', 'soil_temperature_level_3', 'soil_temperature_level_4', 'volumetric_soil_water_layer_1', 'volumetric_soil_water_layer_2', 'volumetric_soil_water_layer_3', 'volumetric_soil_water_layer_4'

<br>

## 2. Download ERA5 Data by Python
We provide Python scripts Download_ERA5-3D.py and Download_ERA5-2D.py to help automate the data download process.
* [Download_ERA5-3D.py](Download_ERA5-3D.py.py)
* [Download_ERA5-2D.py](Download_ERA5-2D.py.py)

<br>

## 3. WPS
####    a) Run geogrid.exe as usual
####    b) ungrib.exe
```
Edit  namelist.wps: interval_seconds = 3600
Run   mpirun -np 8 ./geogrid.exe
Edit  namelist.wps: prefix = '3D',
Link  ln -sf ungrib/Variable_Tables/Vtable.ERA-interim.pl Vtable
Link  pressure levels (PL) data ./link_grib.csh ../ERA5_data/ERA5*3D.grib
Run   mpirun -np 8 ./ungrib.exe
Edit  namelist.wps: prefix = '2D',
Link  ln -sf ungrib/Variable_Tables/Vtable.ERA-interim.ml Vtable
Link  surface (SFC) data ./link_grib.csh ../ERA5_data/ERA5*2D.grib
Run   mpirun -np 8 ./ungrib.exe
```

####    c) metgrid.exe
    Edit  namelist.wps in the metgrid section: fg_name = '3D','2D'
    Run   mpirun -np 8 ./metgrid.exe

<br>

## 4. WRF
    Edit  namelist.input in time_control section: interval_seconds = 3600
    Edit  namelist.input in domain section: e_vert = 40
    Edit  namelist.input in domain section: num_metgrid_levels = 38
    Edit  namelist.input in physics section: surface_input_source = 1

# Run-WRF-model-driven-by-ERA5
Driving WRF with ERA5 data

1. Required Fields
  3D Data (e.g. data on pressure levels)：
  'geopotential', 'relative_humidity', 'specific_humidity', 'temperature', 'u_component_of_wind', 'v_component_of_wind'
  2D Data：
  'surface_pressure', 'mean_sea_level_pressure', '10m_u_component_of_wind', '10m_v_component_of_wind', '2m_temperature', 'sea_surface_temperature', 'skin_temperature', '2m_dewpoint_temperature', 'snow_depth', 'sea_ice_cover', 'land_sea_mask', 'soil_type', 
  'soil_temperature_level_1', 'soil_temperature_level_2', 'soil_temperature_level_3', 'soil_temperature_level_4', 'volumetric_soil_water_layer_1', 'volumetric_soil_water_layer_2', 'volumetric_soil_water_layer_3', 'volumetric_soil_water_layer_4'
  
2. DOWNLOAD ERA5 DATA USING PYTHON
  Download_ERA5-3D.py
  Download_ERA5-2D.py

3. WPS
   1) Run geogrid.exe as usual.
   2) ungrib.exe
      Edit namelist.wps: interval_seconds = 3600
      Run mpirun -np 8 ./geogrid.exe
      Edit namelist.wps: prefix = '3D',
      Link ln -sf ungrib/Variable_Tables/Vtable.ERA-interim.pl Vtable
      Link pressure levels (PL) data ./link_grib.csh ../ERA5_data/ERA5*3D.grib
      Run mpirun -np 8 ./ungrib.exe
      Edit namelist.wps: prefix = '2D',
      Link ln -sf ungrib/Variable_Tables/Vtable.ERA-interim.ml Vtable
      Link surface (SFC) data ./link_grib.csh ../ERA5_data/ERA5*2D.grib
      Run mpirun -np 8 ./ungrib.exe
  3) metgrid.exe
      Edit namelist.wps in the metgrid section: fg_name = '3D','2D'
      Run mpirun -np 8 ./metgrid.exe

4. WRF
  Edit namelist.input in time_control section: interval_seconds = 3600
  Edit namelist.input in domain section: e_vert = 40
  Edit namelist.input in domain section: num_metgrid_levels = 38
  Edit namelist.input in physics section: surface_input_source = 1

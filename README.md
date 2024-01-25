# Run-WRF-model-driven-by-ERA5
Driving WRF with ERA5 data

1. DOWNLOAD ERA5 DATA USING PYTHON

2. WPS
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
Edit namelist.wps in the metgrid section: fg_name = '3D','2D'
Run mpirun -np 8 ./metgrid.exe

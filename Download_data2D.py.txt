#!/usr/bin/python3

import cdsapi

c = cdsapi.Client()

c.retrieve(
    'reanalysis-era5-single-levels',
    {
        'product_type': 'reanalysis',
        'format': 'grib',
        'time': [
            '00:00', '01:00', '02:00',
            '03:00', '04:00', '05:00',
            '06:00', '07:00', '08:00',
            '09:00', '10:00', '11:00',
            '12:00', '13:00', '14:00',
            '15:00', '16:00', '17:00',
            '18:00', '19:00', '20:00',
            '21:00', '22:00', '23:00',
        ],
        'day': [
            '01', '02', '03',
            '04', '05', '06',
            '07', '08', '09',
            '10', '11', '12',
            '13', '14', '15',
        ],
        'month': '07',
        'year': '2022',
        'area': [
            50.00, 70.00, 12.00,
            130.00,
        ],
        'variable': [
            '10m_u_component_of_wind', '10m_v_component_of_wind', '2m_dewpoint_temperature',
            '2m_temperature', 'forecast_albedo', 'forecast_logarithm_of_surface_roughness_for_heat',
            'high_cloud_cover', 'land_sea_mask', 'low_cloud_cover',
            'mean_sea_level_pressure', 'medium_cloud_cover', 'sea_ice_cover',
            'sea_surface_temperature', 'skin_temperature', 'snow_albedo',
            'snow_depth', 'snowfall', 'soil_temperature_level_1',
            'soil_temperature_level_2', 'soil_temperature_level_3', 'soil_temperature_level_4',
            'soil_type', 'surface_pressure', 'total_cloud_cover',
            'total_column_water', 'total_column_water_vapour', 'total_precipitation',
            'volumetric_soil_water_layer_1', 'volumetric_soil_water_layer_2', 'volumetric_soil_water_layer_3',
            'volumetric_soil_water_layer_4',
        ],
    },
    './ERA5_data/ERA5-2022-07-01to15_2D.grib')
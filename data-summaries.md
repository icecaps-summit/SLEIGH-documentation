# Data Summaries {.unnumbered}

This chapter will describe the summarization process applied to the data, prior to its transfer via Iridium satellite network to SANTA (**S**LEIGH **A**ccess **N**ode for data **T**ransfer and **A**nalysis).

**Note:** the data summarisation is a work in process. If a particular instrument isn't described, or the variables for a given instrument won't be sufficient to assess its performance, get in touch with \[who's details, if anyone's, shall we give out on a public website...\].

## Notation

The variables reorded by the instruments will be described in tables below, and the descriptions for the reducing of the data will take the form:

+ The variable name will be described in the `var` collumn, as it is given in the .nc, .dat or .csv files onboard iceman.

+ The dimensions for each variable will be described in the `dims` collumn. For each instrument, the resolution and length of the original dimensions will be described.

+ The data reductions will aggregate data along these dimensions to produce new dimensions, described for each variable in `new_dims`. The resolution and size of these dimensions will also be described.

+ The function(s) used to aggregate the data from `dims` to `new_dims` will be described in the collumn `fn`.

+ A brief description of the use of these reduced variables will be given in the `details` collumn, as well as any other supporting information.


## cl61

### Dimensions
| dimension | resolution | length |
|-----------|------------|--------|
| time      | *FILL*     | (unlimitted) |
| layer     | 1 | 5 |
| time'     | 15 minutes | 96 per day |
: {.striped .bordered}

### Variables
| var | dims | new_dims | fn | units | details
|-|-|-|-|-|------|
| cloud_base_heights | time, layer | time', layer | mean, std | m | The 15-minute mean and standard deviation of the cloud base height for each detected cloud layer |
| cloud_thickness | time, layer | time', layer | mean, std | m | The 15-minute mean and standard deviation of the cloud geometric thickness for each detected cloud layer |
| beta_att_sum | time | time' | median, std | $10^{-4}$ sr$^{-1}$ | The 15-miunte median and standard deviation of the vertically-integrated backscatter coefficient |
| tilt_angle | time | time' | mean, std | ° | 15-minute mean and standard deviation of the instrument zenith angle (from vertical) |
| precipitation_detection | time | time' | sum | 1 | The number of recorded profiles in which precipitation was detected |
| fog_detection | time | time' | sum | 1 | The number of recorded profiles within which fog was detected |
| time | time | time' | count | 1 | The number of recorded profiles per 15-minute window. A proxy for instrument uptime |
: {.striped .bordered}

::: {.callout-note collapse="true" appearance="minimal"}
### ncdump
```
ncdump -h live_20240214_144450.nc

netcdf live_20240214_144450 {
dimensions:
	range = 3276 ;
	layer = 5 ;
	time = UNLIMITED ; // (5 currently)
variables:
	int cloud_base_heights(time, layer) ;
		cloud_base_heights:_FillValue = -99 ;
		cloud_base_heights:units = "m" ;
		cloud_base_heights:long_name = "heights (range) of the detected cloud bases" ;
		cloud_base_heights:coordinates = "time layer longitude latitude" ;
	int vertical_visibility(time) ;
		vertical_visibility:_FillValue = -99 ;
		vertical_visibility:units = "m" ;
		vertical_visibility:long_name = "visibility in the direction of the instrument beam" ;
		vertical_visibility:coordinates = "time longitude latitude" ;
	float p_pol(time, range) ;
		p_pol:_FillValue = -999.f ;
		p_pol:units = "1/(m*sr)" ;
		p_pol:long_name = "parallel-polarized component of the backscattered light" ;
		p_pol:coordinates = "time range longitude latitude" ;
		p_pol:averaging_time_in_seconds = 60 ;
	float x_pol(time, range) ;
		x_pol:_FillValue = -999.f ;
		x_pol:units = "1/(m*sr)" ;
		x_pol:long_name = "cross-polarized component of the backscattered light" ;
		x_pol:coordinates = "time range longitude latitude" ;
		x_pol:averaging_time_in_seconds = 60 ;
	float beta_att(time, range) ;
		beta_att:_FillValue = -999.f ;
		beta_att:units = "1/(m*sr)" ;
		beta_att:long_name = "attenuated volume backscatter coefficient" ;
		beta_att:coordinates = "time range longitude latitude" ;
		beta_att:averaging_time_in_seconds = 60 ;
	float linear_depol_ratio(time, range) ;
		linear_depol_ratio:_FillValue = -999.f ;
		linear_depol_ratio:long_name = "linear depolarisation ratio of the backscatter volume" ;
		linear_depol_ratio:coordinates = "time range longitude latitude" ;
		linear_depol_ratio:averaging_time_in_seconds = 60 ;
	double time(time) ;
		time:_FillValue = -999. ;
		time:units = "seconds since 1970-01-01 00:00:00.000" ;
		time:long_name = "Time" ;
		time:axis = "T" ;
		time:standard_name = "time" ;
		time:cf_role = "profile_id" ;
		time:comment = "represents the end of the averaging period" ;
	double range(range) ;
		range:_FillValue = -999. ;
		range:units = "m" ;
		range:long_name = "measurement distance from the instrument in the direction of the transmitted laser beam" ;
		range:axis = "Z" ;
		range:positive = "up" ;
	int layer(layer) ;
		layer:_FillValue = -999 ;
		layer:units = "layer" ;
		layer:long_name = "number of the observed cloud layer (1,2,...,5)" ;
	double longitude ;
		longitude:_FillValue = -999. ;
		longitude:units = "degrees_east" ;
		longitude:long_name = "longitude" ;
		longitude:standard_name = "longitude" ;
	double latitude ;
		latitude:_FillValue = -999. ;
		latitude:units = "degrees_north" ;
		latitude:long_name = "latitude" ;
		latitude:standard_name = "latitude" ;
	int elevation ;
		elevation:_FillValue = -999 ;
		elevation:units = "m" ;
		elevation:long_name = "elevation" ;
		elevation:standard_name = "ground_level_altitude" ;
		elevation:comment = "measurement site height above or below a fixed reference point, most commonly a reference geoid" ;
	double azimuth_angle ;
		azimuth_angle:_FillValue = -999. ;
		azimuth_angle:units = "degrees" ;
		azimuth_angle:long_name = "measurement azimuth angle" ;
		azimuth_angle:standard_name = "sensor_azimuth_angle" ;
		azimuth_angle:comment = "reference direction: north" ;
	double beta_att_sum(time) ;
		beta_att_sum:_FillValue = -999. ;
		beta_att_sum:units = "1/(10^4*sr)" ;
		beta_att_sum:long_name = "scaled integral of the attenuated volume backscatter coefficient" ;
	double beta_att_noise_level(time) ;
		beta_att_noise_level:_FillValue = -999. ;
		beta_att_noise_level:long_name = "a unitless number describing the noise level of the attenuated volume backscatter coefficient" ;
	short tilt_correction(time) ;
		tilt_correction:_FillValue = -999s ;
		tilt_correction:long_name = "tilt correction" ;
		tilt_correction:comment = "on/off (1/0)" ;
	float tilt_angle(time) ;
		tilt_angle:_FillValue = -999.f ;
		tilt_angle:units = "degrees" ;
		tilt_angle:standard_name = "zenith_angle" ;
		tilt_angle:long_name = "instrument tilt angle from the vertical" ;
	short height_offset(time) ;
		height_offset:_FillValue = -999s ;
		height_offset:long_name = "instrument height offset to reference level" ;
		height_offset:comment = "positive, if the instrument is placed e.g. on the roof of a building. Negative, if the instrument is placed below the ground level altitude e.g. in a pit. This value will be added to the cloud base height results." ;
		height_offset:units = "m" ;
	short airplane_filter_max_range ;
		airplane_filter_max_range:_FillValue = -999s ;
		airplane_filter_max_range:units = "m" ;
		airplane_filter_max_range:long_name = "airplane filter max range" ;
		airplane_filter_max_range:comment = "user configured value, zero for not in use, otherwise the configured range" ;
	short sky_condition_total_cloud_cover(time) ;
		sky_condition_total_cloud_cover:_FillValue = -99s ;
		sky_condition_total_cloud_cover:units = "oktas" ;
		sky_condition_total_cloud_cover:long_name = "total amount of cloud cover" ;
		sky_condition_total_cloud_cover:comment = "aggregated across layers" ;
		sky_condition_total_cloud_cover:coordinates = "time longitude latitude" ;
	short sky_condition_cloud_layer_covers(time, layer) ;
		sky_condition_cloud_layer_covers:_FillValue = -99s ;
		sky_condition_cloud_layer_covers:units = "oktas" ;
		sky_condition_cloud_layer_covers:long_name = "amount of cloud cover in different cloud layers" ;
		sky_condition_cloud_layer_covers:comment = "for up to 5 layers" ;
		sky_condition_cloud_layer_covers:coordinates = "time layer longitude latitude" ;
	int sky_condition_cloud_layer_heights(time, layer) ;
		sky_condition_cloud_layer_heights:_FillValue = -99 ;
		sky_condition_cloud_layer_heights:units = "m" ;
		sky_condition_cloud_layer_heights:long_name = "height of different cloud layers" ;
		sky_condition_cloud_layer_heights:comment = "for up to 5 layers" ;
		sky_condition_cloud_layer_heights:coordinates = "time layer longitude latitude" ;
	int cloud_penetration_depth(time, layer) ;
		cloud_penetration_depth:_FillValue = -99 ;
		cloud_penetration_depth:units = "m" ;
		cloud_penetration_depth:long_name = "cloud penetration depth in the direction of the instrument beam" ;
		cloud_penetration_depth:coordinates = "time layer longitude latitude" ;
	int cloud_thickness(time, layer) ;
		cloud_thickness:_FillValue = -99 ;
		cloud_thickness:units = "m" ;
		cloud_thickness:long_name = "cloud thickness in the direction of the instrument beam" ;
		cloud_thickness:coordinates = "time layer longitude latitude" ;
	short precipitation_detection(time) ;
		precipitation_detection:_FillValue = -999s ;
		precipitation_detection:long_name = "detection of ground reaching precipitation" ;
		precipitation_detection:comment = "detected/not-detected (1/0)" ;
		precipitation_detection:coordinates = "time longitude latitude" ;
	short fog_detection(time) ;
		fog_detection:_FillValue = -999s ;
		fog_detection:long_name = "detection of fog" ;
		fog_detection:comment = "detected/not-detected (1/0)" ;
		fog_detection:coordinates = "time longitude latitude" ;
	short receiver_gain(time) ;
		receiver_gain:_FillValue = -999s ;
		receiver_gain:long_name = "receiver gain status" ;
		receiver_gain:comment = "high-gain/low-gain (1/0)" ;
	float range_resolution ;
		range_resolution:_FillValue = -999.f ;
		range_resolution:units = "m" ;
		range_resolution:long_name = "range resolution" ;
		range_resolution:comment = "distance between consecutive profile elements" ;
	double cloud_calibration_factor ;
		cloud_calibration_factor:_FillValue = -999. ;
		cloud_calibration_factor:long_name = "factory cloud calibration value" ;
		cloud_calibration_factor:comment = "instrument specific beta_att calibration value measured at the factory" ;
	double cloud_calibration_factor_user ;
		cloud_calibration_factor_user:_FillValue = -999. ;
		cloud_calibration_factor_user:long_name = "user set cloud calibration value" ;
		cloud_calibration_factor_user:comment = "instrument specific beta_att calibration value set by the user, same as the factory value by default" ;
	float overlap_function(range) ;
		overlap_function:_FillValue = -999.f ;
		overlap_function:long_name = "instrument specific overlap function" ;
		overlap_function:comment = "shares the vertical resolution of profiles" ;

// global attributes:
		:title = "CL61D CL61 with Depolarization" ;
		:institution = "" ;
		:source = "" ;
		:conventions = "CF-1.8" ;
		:schema_version = "1.3" ;
		:sw_version = "1.2.7" ;
		:history = "" ;
		:comment = "" ;
		:unit = "m" ;
		:instrument_serial_number = "U1510972" ;
		:overlap_function_provided = 1s ;
		:overlap_is_corrected = 1s ;
		:file_temporal_span_in_minutes = 5. ;
		:profile_interval_in_seconds = 60 ;

group: monitoring {
  variables:
  	double time(time) ;
  		time:_FillValue = -999. ;
  		time:units = "seconds since 1970-01-01 00:00:00.000" ;
  		time:long_name = "Time" ;
  		time:axis = "T" ;
  		time:standard_name = "time" ;
  	float window_condition(time) ;
  		window_condition:_FillValue = -999.f ;
  		window_condition:units = "percent" ;
  		window_condition:long_name = "window condition" ;
  		window_condition:comment = "100 for a clean, 0 for a totally dirty window" ;
  	float laser_power_percent(time) ;
  		laser_power_percent:_FillValue = -999.f ;
  		laser_power_percent:units = "percent" ;
  		laser_power_percent:long_name = "laser power percent" ;
  	float background_radiance(time) ;
  		background_radiance:_FillValue = -999.f ;
  		background_radiance:long_name = "background radiance" ;
  		background_radiance:range = "[0 1747]" ;
  	float internal_temperature(time) ;
  		internal_temperature:_FillValue = -999.f ;
  		internal_temperature:units = "celsius" ;
  		internal_temperature:long_name = "internal temperature" ;
  	float internal_humidity(time) ;
  		internal_humidity:_FillValue = -999.f ;
  		internal_humidity:units = "RH" ;
  		internal_humidity:long_name = "internal humidity" ;
  		internal_humidity:comment = "percent (% RH)" ;
  	float internal_pressure(time) ;
  		internal_pressure:_FillValue = -999.f ;
  		internal_pressure:units = "hPa" ;
  		internal_pressure:long_name = "internal pressure" ;
  	float laser_temperature(time) ;
  		laser_temperature:_FillValue = -999.f ;
  		laser_temperature:units = "celsius" ;
  		laser_temperature:long_name = "laser temperature" ;
  	float window_blower(time) ;
  		window_blower:_FillValue = -999.f ;
  		window_blower:long_name = "window blower" ;
  		window_blower:comment = "on/off (1/0)" ;
  	float internal_heater(time) ;
  		internal_heater:_FillValue = -999.f ;
  		internal_heater:long_name = "internal heater" ;
  		internal_heater:comment = "on/off (1/0)" ;
  	float window_blower_heater(time) ;
  		window_blower_heater:_FillValue = -999.f ;
  		window_blower_heater:long_name = "window blower heater" ;
  		window_blower_heater:comment = "on/off (1/0)" ;
  	float transmitter_enclosure_temperature(time) ;
  		transmitter_enclosure_temperature:_FillValue = -999.f ;
  		transmitter_enclosure_temperature:units = "celsius" ;
  		transmitter_enclosure_temperature:long_name = "transmitter enclosure temperature" ;
  } // group monitoring

group: status {
  variables:
  	double time(time) ;
  		time:_FillValue = -999. ;
  		time:units = "seconds since 1970-01-01 00:00:00.000" ;
  		time:long_name = "Time" ;
  		time:axis = "T" ;
  		time:standard_name = "time" ;
  	short Device_controller_temperature(time) ;
  		Device_controller_temperature:_FillValue = -999s ;
  		Device_controller_temperature:long_name = "Device_controller_temperature" ;
  	short Device_controller_electronics(time) ;
  		Device_controller_electronics:_FillValue = -999s ;
  		Device_controller_electronics:long_name = "Device_controller_electronics" ;
  	short Device_controller_overall(time) ;
  		Device_controller_overall:_FillValue = -999s ;
  		Device_controller_overall:long_name = "Device_controller_overall" ;
  	short Optics_unit_accelerometer(time) ;
  		Optics_unit_accelerometer:_FillValue = -999s ;
  		Optics_unit_accelerometer:long_name = "Optics_unit_accelerometer" ;
  	short Optics_unit_electronics(time) ;
  		Optics_unit_electronics:_FillValue = -999s ;
  		Optics_unit_electronics:long_name = "Optics_unit_electronics" ;
  	short Optics_unit_overall(time) ;
  		Optics_unit_overall:_FillValue = -999s ;
  		Optics_unit_overall:long_name = "Optics_unit_overall" ;
  	short Optics_unit_memory(time) ;
  		Optics_unit_memory:_FillValue = -999s ;
  		Optics_unit_memory:long_name = "Optics_unit_memory" ;
  	short Optics_unit_tilt_angle(time) ;
  		Optics_unit_tilt_angle:_FillValue = -999s ;
  		Optics_unit_tilt_angle:long_name = "Optics_unit_tilt_angle" ;
  	short Receiver_electronics(time) ;
  		Receiver_electronics:_FillValue = -999s ;
  		Receiver_electronics:long_name = "Receiver_electronics" ;
  	short Receiver_overall(time) ;
  		Receiver_overall:_FillValue = -999s ;
  		Receiver_overall:long_name = "Receiver_overall" ;
  	short Receiver_memory(time) ;
  		Receiver_memory:_FillValue = -999s ;
  		Receiver_memory:long_name = "Receiver_memory" ;
  	short Receiver_voltage(time) ;
  		Receiver_voltage:_FillValue = -999s ;
  		Receiver_voltage:long_name = "Receiver_voltage" ;
  	short Receiver_solar_saturation(time) ;
  		Receiver_solar_saturation:_FillValue = -999s ;
  		Receiver_solar_saturation:long_name = "Receiver_solar_saturation" ;
  	short Window_blocking(time) ;
  		Window_blocking:_FillValue = -999s ;
  		Window_blocking:long_name = "Window_blocking" ;
  	short Window_condition(time) ;
  		Window_condition:_FillValue = -999s ;
  		Window_condition:long_name = "Window_condition" ;
  	short Window_blower_fan(time) ;
  		Window_blower_fan:_FillValue = -999s ;
  		Window_blower_fan:long_name = "Window_blower_fan" ;
  	short Window_blower_heater(time) ;
  		Window_blower_heater:_FillValue = -999s ;
  		Window_blower_heater:long_name = "Window_blower_heater" ;
  	short Servo_drive_electronics(time) ;
  		Servo_drive_electronics:_FillValue = -999s ;
  		Servo_drive_electronics:long_name = "Servo_drive_electronics" ;
  	short Servo_drive_overall(time) ;
  		Servo_drive_overall:_FillValue = -999s ;
  		Servo_drive_overall:long_name = "Servo_drive_overall" ;
  	short Servo_drive_memory(time) ;
  		Servo_drive_memory:_FillValue = -999s ;
  		Servo_drive_memory:long_name = "Servo_drive_memory" ;
  	short Servo_drive_control(time) ;
  		Servo_drive_control:_FillValue = -999s ;
  		Servo_drive_control:long_name = "Servo_drive_control" ;
  	short Servo_drive_ready(time) ;
  		Servo_drive_ready:_FillValue = -999s ;
  		Servo_drive_ready:long_name = "Servo_drive_ready" ;
  	short Transmitter_electronics(time) ;
  		Transmitter_electronics:_FillValue = -999s ;
  		Transmitter_electronics:long_name = "Transmitter_electronics" ;
  	short Transmitter_light_source(time) ;
  		Transmitter_light_source:_FillValue = -999s ;
  		Transmitter_light_source:long_name = "Transmitter_light_source" ;
  	short Transmitter_light_source_power(time) ;
  		Transmitter_light_source_power:_FillValue = -999s ;
  		Transmitter_light_source_power:long_name = "Transmitter_light_source_power" ;
  	short Transmitter_overall(time) ;
  		Transmitter_overall:_FillValue = -999s ;
  		Transmitter_overall:long_name = "Transmitter_overall" ;
  	short Transmitter_light_source_safety(time) ;
  		Transmitter_light_source_safety:_FillValue = -999s ;
  		Transmitter_light_source_safety:long_name = "Transmitter_light_source_safety" ;
  	short Transmitter_memory(time) ;
  		Transmitter_memory:_FillValue = -999s ;
  		Transmitter_memory:long_name = "Transmitter_memory" ;
  	short Maintenance_overall(time) ;
  		Maintenance_overall:_FillValue = -999s ;
  		Maintenance_overall:long_name = "Maintenance_overall" ;
  	short Device_overall(time) ;
  		Device_overall:_FillValue = -999s ;
  		Device_overall:long_name = "Device_overall" ;
  	short Recently_started(time) ;
  		Recently_started:_FillValue = -999s ;
  		Recently_started:long_name = "Recently_started" ;
  	short Measurement_status(time) ;
  		Measurement_status:_FillValue = -999s ;
  		Measurement_status:long_name = "Measurement_status" ;
  	short Datacom_overall(time) ;
  		Datacom_overall:_FillValue = -999s ;
  		Datacom_overall:long_name = "Datacom_overall" ;
  	short Measurement_data_destination_not_set(time) ;
  		Measurement_data_destination_not_set:_FillValue = -999s ;
  		Measurement_data_destination_not_set:long_name = "Measurement_data_destination_not_set" ;
  	short Inside_heater(time) ;
  		Inside_heater:_FillValue = -999s ;
  		Inside_heater:long_name = "Inside_heater" ;
  	short Receiver_sensitivity(time) ;
  		Receiver_sensitivity:_FillValue = -999s ;
  		Receiver_sensitivity:long_name = "Receiver_sensitivity" ;
  	short Data_generation_status(time) ;
  		Data_generation_status:_FillValue = -999s ;
  		Data_generation_status:long_name = "Data_generation_status" ;
  } // group status
}
```
:::


## MWR 

### Dimensions

| dimension | resolution | length |
|-|-|-|
| time | *FILL* | (unlimitted) |
| number_frequencies | 1 | 8 |
| time' | 15 minutes | 96 per day |
: {.striped .bordered}


### Variables

#### BRT

| var | dims | new_dims | fn | units | details |
|-|-|-|-|-|-----|
| time | time | time' | count | 1 | The number of data records every 15 minutes. Proxy for instrument uptime |
| RF | time | time' | sum | 1 | The number of data points where the rain flag is present |
| TBs | time, number_frequencies | time', number_frequencies | mean, std | K | 15-minute mean and standard deviation of the brightness temperatures at each recorded frequency |
: {.striped .bordered}

#### HKD

| var | dims | new_dims | fn | units | details |
|-|-|-|-|-|-----|
| time | time | time' | count | 1 | The number of data records every 15 minutes, proxy for instrument uptime |
| ALFL | time | time' | sum | 1 | Sum of **AL**arm **FL**ags, indicating critical system status |
| AT1_T | time | time' | mean, std | K | Ambient target 1 temperature |
| AT2_T | time | time' | mean, std | K | Ambient target 2 termperature |
| Rec1_T | time | time' | mean, std | K | Receiver 1 temperature |
| Rec2_T | time | time' | mean, std | K | Receiver 2 temperature |
| Rec1_Stab | time | time' | mean, std | K | Receiver 1 temperature stability |
| Rec2_Stab | time | time' | mean, std | K | Receiver 2 temperature stability |

: {.striped .bordered}

#### MET

| var | dims | new_dims | fn | units | details |
|-|-|-|-|-|-----|
| time | time | time' | count | 1 | The number of data records every 15 minutes, proxy for instrument uptime |
| Surf_P | time | time' | mean, std | hPa | 15-minute mean and standard deviation of surface pressure |
| Surf_T | time | time' | mean, std | K | 15-minute mean and standard deviation of surface temperature |
| Surf_RH | time | time' | mean, std | % | 15-minute mean and standard deviation of surface relative humidity |
| RF |  time | time' | sum | 1 | The number of data points where the rain flag is present, should match RF from the BRT data stream |
: {.striped .bordered}


## Energymeter

### Dimensions
| dimension | resolution | length |
|-----------|------------|--------|
| time      | *FILL*     | (unlimitted) |
| time'     | 15 minutes | 96 per day |
: {.striped .bordered}

### Variables
| var | dims | new_dims | fn | units | details
|-|-|-|-|-|------|
| time | time | time' | count | 1 | The number of data records per 15 minute period, proxy for instrument uptime |
| ac_voltage | time | time' | mean, std, min, max, median | V | Statistics for the 15-minute distribution of instantaneous supplied voltage |
| ac_current | time | time' | mean, std, min, max, median | A | Statistics for the 15-minute distribution of instantaneous supplied current |
| ac_active_power | time | time' | mean, std, min, max, median | W | Statistics for the 15-minute distribution of instantaneous supplied current | 
: {.striped .bordered}





## MRR

### Dimensions
| dimension | resolution | length |
|-----------|------------|--------|
| time      | *FILL, 10 s?*     | (unlimitted) |
| range   | 35 m | 128 |
| time'     | 15 minutes | 96 per day |
| range' | 280 m | 16 |
: {.striped .bordered}

### Variables
| var | dims | new_dims | fn | units | details
|-|-|-|-|-|------|
| time | time | time' | count | 1 | Number of data records per 15 minute period, proxy for instrument uptime |
| Z | time, range | time', range' | median, std | dBZ | Log-reflectivity |
| VEL | time, range | time', range' | median, std | m s$^{-1}$ | Doppler velocity |
| WIDTH | time, range | time', range' | median, std |m s$^{-1}$ | Doppler spectrum width |
| RR | time, range | time' | mean | mm hr$^{-1}$ | Mean rainfall rate throughout the collumn |
| LWC | time, range | time' | mean( sum\[range\] ) | g m$^{-2}$ | Mean liquid water path |
: {.striped .bordered}

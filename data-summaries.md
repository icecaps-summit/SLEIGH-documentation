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

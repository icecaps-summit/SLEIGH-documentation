# Data dashboard {.unnumbered}

The data dashboard for the ICECAPS MELT deployment can be found at [icecapsmelt.org/dashboard](http://www.icecapsmelt.org/dashboard).


## Code structure


## Panel server

The dashboard runs off of the python module `panel` to render the plots and allow interactivity. Because of this, it requires a `bokeh` server to be running in order to allow the serving of non-static html pages (i.e. allows loading of the most recent data, and interactovoty).

This section will explain how the `bokeh` server can be initialised from SANTA, either for the purposes of testing changes to the dashboard, or for deploying the data dashboard to the live website.

### Testing \[Pre deployment\]




### Deployment

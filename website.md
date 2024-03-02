# Website {.unnumbered}

SANTA hosts the website [icecapsmelt.org](http://www.icecapsmelt.org), from which the data dashboard and documentation will be available.


## Landing page

The landing page (currently very very much a work in progress) will be rendered from the [icecaps-summit:icecapsmeltorg](https://github.com/icecaps-summit/icecapsmeltorg) under the `landing_page` directory.

The page will be rendered via quarto as a simple `index.html` file which can be copied into the root of the website directory.


## Data dashboard


## Documentation

The latest live version of the documentation can be found at [icecapsmelt.org/documentation](http://www.icecapsmelt.org/documentation/). The documentation is updated live every 15 minutes, pulling from the `main` branch of the github repository `sleigh-documentation`.

The service to automatically update the live documentation pages can be found at the github repo [icecaps-summit:icecapsmeltorg](https://github.com/icecaps-summit/icecapsmeltorg).

The service consists of a timer and service file. The timer file triggers on systemd every 15 minutes (with a random 5 minute extension to this) and will call to the service file. This then runs the bash script specified in the repository given above. The script simply pulls in the latest version of the documentation from the main branch, replaces text in the `_quarto-production.yml` to state when the render occurred and for which git commit, renders the website using quarto and then copies it to the website directories.



Boop beep
====================
Beep boop
(this is a test of the auto-updating website...)
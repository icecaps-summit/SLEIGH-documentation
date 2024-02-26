# SANTA {.unnumbered}

The **S**leigh **A**ccess **N**ode for data **T**ransfer and **A**nalysis (SANTA) is a VM set up as part of the ICECAPS external tenancy on the NERC's JASMIN computer.


## Access


## Data Storage



## Data Transfer


## Website Hosting

We have a website! [http://www.icecapsmelt.org](http://www.icecapsmelt.org)

### Dashboard Update Pipeline

1. Perform `git pull` on dashboard repo to obtain the latest data visualisation code.
2. Ensure that a `data_dir.txt` file in the root of the repo exists, and is contained in the repo's `.gitignore`. This will allow different users of the repository to securely link to locally stored data.

```{filename='e.g. example data_dir.txt on SANTA'}
/data
```

3. `quarto render` will render the dashboard, which will use `data_dir.txt` to point to the most recently generated data. The render location can be set to the hosted website's location on SANTA. This will automatically update a static quarto render to display the latest available data.

There are a few considerations to implement this procedure:
- We need to decide on a timeframe after which the `quarto render` is reran to keep the website up to date. 15 minutes would ensure that as a minimum time delay between a data downlink and it being publicly viewable.
- We need a way to ensure that data isn't read as its being written from the SLEIGH. This would be disasterous, as the data partition that SANTA uses is no-parallel-write and this could corrupt data if read and write calls are made at the same time.

#### Solution #1
Push the data to a seperate directory (e.g. `/data/current_downlink`), from which a simple `mv` command is applied, only once the data has been safely downloaded and checked.
```{filename='e.g. data transfer pipeline'}
data on SLEIGH -> /data/current_downlink on SANTA -> /data/hourly_summaries on SANTA
                ^                                 ^
              Iridium                            mv
```
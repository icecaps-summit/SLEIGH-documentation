# SANTA {.unnumbered}

The **S**leigh **A**ccess **N**ode for data **T**ransfer and **A**nalysis (SANTA) is a VM set up as part of the ICECAPS external tenancy on the NERC's JASMIN computer.

SANTA can be accessed at the ip address `192.171.169.156`, please ensure that you have an account with a specified ssh key setup beforehand. In order to set this up, please contact Andrew Martin, or Ryan Neely.



## Data Transfer

### pre-deployment

Currently, data transfer happens from the SLEIGH to SANTA from the MVPi, and after the daily summaries have been created, tarred and subsequently chunked.

### File compression and chunking \[SLEIGH\]

Prior to being sent, the summarised `.nc` files and summary log `.txt` files are tarred together using the `tar` utility. The specific command used to achieve this is:

```bash
txzc=$(tar -c -I "xz -9 -T0" -f $argv[1] $argv[2..-1])
```

This runs the tar compression using the `xz` compression algoriithm, with the highest compression flag (`-9`). The file provided as the first argument to `txzc` will be the created archive, and the folloowing arguments are the files to be tarred.

The `split` utility is then used to reduce the file size further so that any individual file sent is only 5 KiB in size:

```bash
split --suffix-length=3 --bytes=5K $argv[1] split/$argv[1]. --verbose
```

This will create 5 KiB files of the name `split/$argv[1].[aaa-zzz]`, incrementing the three character suffix until enough files have been created to contain all the data present in the orginal data.

The files are then ready to be transferred to SANTA, either via rsync (pre-deployment) or via **\[INSERT METHOD HERE FOR IRIDIUM TRANSFER\]**

### File merging and uncompression \[SANTA\]

Once the files have arrived on SANTA, they can be found in the directory `/data/daily_summaries/YYYYMMDD/split`. In order to obtain the original summary files, two commands must be used. 

Firstly, the files need to be merged to create the original tar archive:

```bash
cd /data/daily_summaries/YYYYMMDD
cat split/* > summary.tar
```

Next, the decompression algorithm needs to be performed, specifically using the same compression dictionary settings as were provided in the `txzc` compression command:

```bash
tar -x -I "xz -9 -T0" -f summary.tar
```

This should produce the original files and place them in the directory `/data/daily_summaries/YYYYMMDD`. From here, they can be moved to the appropritae directories for the data dashobard to access.


## Website Hosting

We have a website! [http://www.icecapsmelt.org](http://www.icecapsmelt.org)

### \[PRELIMINARY\] Dashboard Update Pipeline

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
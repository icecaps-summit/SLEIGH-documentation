# Services {.unnumbered}

The instrument platform uses Linux services to manage all the instruments. For instance, services can be used to turn certain instruments off or on. Services can also be scheduled to perform certain tasks, like data transfers or backups.

The operation of Linux services are logged into the system journal, so the system journal is a complete record of how the instrument platform services were used during a particular time period, such as a field experiment. The system journal can be used to determine the history of activity of a particular service.

The services for the instrument platform are stored in

```
/home/iceman/.config/systemd/user
```

Here is a complete list of services for the instrument platform as of 21 May 2023:

```
Permissions Size User   Date Modified Name
.rw-r--r--   172 iceman  1 Mar 04:36  asfssync.service
.rw-r--r--   157 iceman  1 Mar 04:36  asfssync.timer
.rw-r--r--   221 iceman 20 May 11:28  datamansync.service
.rw-r--r--   157 iceman 20 May 11:23  datamansync.timer
drwxr-xr-x     - iceman 21 May 13:27  default.target.wants
.rw-r--r--   467 iceman 22 Jul  2022  emacs.service
.rw-r--r--   326 iceman 17 May 15:26  energymeter.service
.rw-r--r--   415 iceman 20 May 15:17  gpr-data-transfer.service
.rw-r--r--   194 iceman 18 May 19:49  gpr-data-transfer.timer
.rw-r--r--   427 iceman 20 May 11:20  gps-data-collect.service
.rw-r--r--   330 iceman 20 May 01:05  midnight.service
.rw-r--r--   181 iceman 21 May 13:26  midnight.timer
.rw-r--r--   266 iceman 18 May 19:29  mrr-data-transfer.service
.rw-r--r--   186 iceman 18 May 19:32  mrr-data-transfer.timer
.rw-r--r--   314 iceman 19 May 12:58  mrr.service
drwxr-xr-x     - iceman 22 Mar 02:43  multi-user.target.wants
.rw-r--r--   226 iceman 15 Dec  2022  mvp_emulator.service
.rw-r--r--   499 iceman 16 Dec  2022  mwr.service
.rw-r--r--   370 iceman 22 Mar 02:45  phonehome.service
.rw-r--r--  1.8k iceman  1 Mar 01:05  qemu@.service
.rw-r--r--   572 iceman 21 May 13:19  simba-data-collect.service
.rw-r--r--   365 iceman 19 May 12:58  cl61.service
.rw-r--r--   586 iceman 16 May 22:29  vnc.service
```

To view any of these services while logged into the Cincoze computer, type

```
# For instance, asfssync.service
less /home/iceman/.config/systemd/user/asfssync.service
```

Typically, services run scripts on the Cincoze to accomplish certain tasks. Scripts written for the instrument platform are stored in

```
/home/iceman/scripts
```

The directory structure for scripts is

```
├── home
│   ├── iceman
│   │   ├── scripts
│   |   |   ├── apc
│   |   |   ├── asfs
│   |   |   ├── blesensors
│   |   |   ├── cams
│   |   |   ├── cruft
│   |   |   ├── datatx
│   |   |   ├── gps
│   |   |   ├── jupyter
│   |   |   ├── misc
│   |   |   ├── mrr
│   |   |   ├── mvp
│   |   |   ├── mwr
│   |   |   ├── platform
│   |   |   ├── simba
│   |   |   ├── cl61
```

:::{.callout-note}
Navigate to this [link](https://www.redhat.com/sysadmin/linux-systemctl-manage-services) to learn more about Linux services.
:::

## \[Placeholder, on iceman\]

+ summarise_yday.service
+ summarise_yday.timer
+ summarise_yday_xfer.service


## Santa services

| unit | description | user/system |
|-----------|------------|--------|
| `website_update_documentation.timer` | Timer that triggers every 15 minues | system |
| `website_update_documentation.service` | Downlaods latest git commit from `icecaps-summit/sleigh-documentation`, renders to website | `website_admin` |
| `website_update_dashboard.timer` | Timer that triggers at the top of eery hour |  system |
| `website_update_dashboard.target` | Target service used to trigger multiple other services simultaneously | system |
| `website_update_dashboard.service` | Downloads latest commit from main branch of `icecaps-summit/sleigh-dashboard` | `website_admin` |
| `website_restart_dashboard.service` | Waits 10s and then restarts `dashboard.service` | system |
| `dashboard.service` | Service that continuously runs a panel bokeh server to serve the data dashboard | `website_admin` |
: {.striped .bordered}

### website_update_documentation.timer

This is a system service that is enabled, and will trigger with the timestamps `*-*-* *:00,15,30,45:00` -- every 15 minutes. The unit that it triggers is `website_update_documentation.service`.

### website_update_documentation.service

This is a system service unit that is enabled and is triggered by `website_update_documentation.timer`. The unit is ran as the user `website_admin`, and it runs a bash script that pulls the latest documentation in the main branch of the documentation github repo, inserts the time of rendering and latest git commit to the footer, renders the website and copies it to the live website location on Santa.

### website_update_dashboard.timer

This is a system service that is enabled, and triggers with the timestamp `*-*-* *:00:00` -- the start of every hour. The unit is triggers is `website_update_dashboard.target`.

### website_update_dashboard.target

This is a target unit created with the intention of allowing multiple systemd services to trigger when `website_update_dashboard.timer` triggers. The target simply installs with `website_update_dashboard.timer`, and uses `wants` and `after` to trigger `website_update_dashboard.service` and `website_restart_dashboard.service`.

### website_update_dashboard.service

This is a oneshot system service unit that is enabled and triggered by `wesite_update_dashboard.target`. It runs as the `website_admin` user, and runs a bash script that simply downloads the latest commit on the main branch of the sleigh-dashboard github repo.

### website_restart_dashboard.service

This is a oneshot system service unit that is enabled and triggered by `website_update_dashboard.target`. It simply waits 10 seconds, which allows `website_update_dashboard.service` to finish executing, and then runs a systemctl restart command to restart `dashboard.service`.

### dashboard.service

This is an always-restarting forking service, ran as `website_admin`, that is enabled upon startup. The service runs the panel data dashboard with the appropriate python environment.

## Using the System Journal

:::{.callout-note}
The system journal is described [here](https://wiki.archlinux.org/title/Systemd/Journal).
:::

Some useful commands for accessing information from the system journal are:

```
journalctl --user -b -u mrr
```

which is an example that lists all the activity of the ```mrr``` service since the last boot, and

```
journalctl --since="2023-05-10 12:00:00" | grep mrr
```

which lists all the activity of the ```mrr``` service since 10 May 2023 at 12 UTC.
# Configuration {.unnumbered}

## Windows Virtual Machine (Windows VM)

To configure certain instruments, it is necessary to login into a Windows virtual machine that is setup on the Cincoze computer. To do this from your local computer, you will use a VNC viewer. Before connecting via VNC, you will need to execute the following command to forward the port from the VM to your local computer:

```
ssh -L 5901:localhost:5901 iceman@192.168.98.50
```

This will create an SSH tunnel from your local computer to the Cincoze computer, and then log you in to the Cincoze. Enter the password for the iceman account, if prompted. Once you're logged in, use a VNC client on your local computer to make a VNC connection to:

```
localhost:5901
```

Enter the password for the iceman account again, if prompted. Your VNC client should then show the desktop of the VM on the Cincoze.

Once you have access to the Windows VM desktop, you will be able to configure the ASFS, MRR and MWR as described below.

### Automated Surface Flux Station (ASFS)

### Micro Rain Radar (MRR)

- How to set "comment" and "site_name" using MRR GUI on VM

### Microwave Radiometer (MWR)

## Command Line Interface (CLI)

### Ground Penetrating Radar (GPR)

### SIMB3 thermistor string (SIMB3)

### Vaisala (CL61) Depolarization Lidar (CL61)

- How to set latitude and longitude using maintenance port via the CLI

## APC Internet-Enabled Power Strip

The instrument platform uses an APC internet-enabled power strip to turn certain instruments on and off. The table below lists the instruments and which port of the power strip it uses. The Cincoze computer has a Linux script that can be used to turn any of the port on or off.

| Port | Instrument       |
|------|------------------|
|  1   |                  |
|  2   |                  |
|  3   |  MWR power       |
|  4   |  MWR heater      |
|  5   |  MRR heater      |
|  6   |  MRR power       |
|  7   |                  |
|  8   |  CL61 power      |

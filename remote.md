# Remote Connection {.unnumbered}

## SSH connection

### Obtain SSH keys

For easy remote login to the platform (Cincoze) computer, one needs two sets of SSH keys; one for dataman and another for iceman (Cincoze). Please contact [Michael Gallagher](mailto:michael.r.gallagher@noaa.gov); he will be able to supply the keys.

### Set up config file for SSH

Add the following to your config file in your ~/.ssh directory on your local computer.

```
Host dataman
    User dataman
    # ....Contact Michael Gallagher for IP address and port number
    Hostname xxx.xxx.xxx.xxx
    Port xxxx
    PubkeyAuthentication yes
    IdentitiesOnly yes
    IdentityFile ~/.ssh/id_rsa_buoy
    ProxyJump gaia

Host iceman
    User iceman
    Hostname 192.168.98.50
    ForwardX11 yes
    ForwardAgent yes
    IdentitiesOnly yes
    IdentityFile ~/.ssh/id_ed_iceman
    ProxyJump dataman
```

```
Host dataman
    User dataman
    # ....Contact Michael Gallagher for IP address and port number
    Hostname xxx.xxx.xxx.xxx
    Port xxxx
    PubkeyAuthentication yes
    IdentitiesOnly yes
    IdentityFile ~/.ssh/id_rsa_buoy
    ProxyJump gaia

Host iceman
  User iceman
  Hostname localhost   
  Port 5901
  ProxyJump oracle
  IdentitiesOnly yes
  IdentityFile ~/.ssh/id_ed_iceman

Host oracle
  User flux
  Hostname 158.101.13.249
  Port 22   
  ForwardAgent yes
  IdentitiesOnly yes
  IdentityFile ~/.ssh/id_ed_iceman

```

:::{.callout-note}
Note that the necessary SSH keys are: 
- ~/.ssh/id_rsa_buoy
- ~/.ssh/id_ed_iceman
:::

:::{.callout-caution}
Note that you will have to set the line ```ProxyJump gaia``` to use the name of your local computer. gaia is Von Walden's linux workstation at WSU.
:::

### Connect to dataman

Using the settings above, one can now connect to dataman by typing:

```
ssh dataman
```

### Connect to iceman (Cincoze)

Using the settings above, note that the connection to iceman uses a ProxyJump to dataman (at Summit). One can connect directly to iceman by typing:

```
ssh iceman
```

## Connecting to the Cincoze VM using vnc

1. Open a terminal window on your local computer and type:

    ```
    ssh -L 5901:localhost:5901 iceman
    ```

    This will set up forwarding from port 5901 to port 5901 on your local computer.

2. Open your VNC client on your local computer and connect to 
   
   ```
   localhost:5901
   ```

3. If the VNC connection works properly, you should be prompted to enter the password for iceman.

List of web servers to connect to using Microsoft Edge Browser

| IP address         | Web Server                     |
|--------------------|--------------------------------|
| 192.168.1.20:8001  | METEK MRR Pro Webcontrol       |
| 192.168.1.20/#Home | MRRPro92: System Configuration |

## Connecting to Web Servers

## Connection to the Jupyter Server

There is a Jupyter Notebook server running on the Cincoze computer. It runs on port 8888, which is the default port used by Jupyter.

1. To avoid, conflicting with a possible Jupyter server on your local computer, type the following command on your local computer to forward the Jupyter server to another port on your local computer:

    ```
    ssh -L 7777:localhost:8888 iceman
    ```

2. To keep this SSH connection active, type ```top``` in the terminal running SSH once you're logged in to iceman. This provides a more responsive web browser experience.

3. Now connect to the Jupyter Server on the Cincoze by navigating your local browser to 

    ```
    localhost:7777
    ```

Please note that this connection can take a long time (minutes). But once you're logged into the Jupyter Server, your browser will be much more responsive.

## Setting up the Windows VM on the Cincoze

```
From Michael G. on 11/29/2023:

There are two ways to use qemu. Without assistance and using libvirt. For the VM I'm using bare qemu without any virtualization management. It's a bit harder to set up (no gui) but it's lower level and much easier to run as a service or modify. There are three key files.

~/.config/systemd/user/qemu@.service
~/.config/qemu/windows.conf
/data/vm/windows_vm.qcow

You start the VM, or shut it down, with "systemctl --user start qemu@windows.service"... the @windows part specifies the config file that's written above. FYI, the config uses a tap-bridge connection to run a "parallel" virtual NIC so that the VM appears as if it were a separate machine on the network. This is much easier than trying to port forward anything you need, although there is a qemu conf file in there that shows how to do port forward stuff. For a laptop that generally works better because you can only run a tap-bridge with an ethernet interface, not wifi.

Finally FYI, something about my tap-bridge config broke with the updated OS and I need to figure that out. I just noticed it. I'll try to fix that soon.

"Connecting" to the VM remotely can be done with vncviewer or with remote desktop. This is pretty straightforward using ssh port forwarding. The VNC server is on the Linux side (192.168.1.10:5901) and the remote desktop is on the VM side once I fix the tap-bridge (192.168.1.166).
```

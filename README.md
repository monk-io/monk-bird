BirdBGP meets Monk
===

This repository contains Monk.io template to deploy Bird BGP system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

- [BirdBGP meets Monk](#birdbgp-meets-monk)
  - [Prerequisites](#prerequisites)
    - [Make sure monkd is running.](#make-sure-monkd-is-running)
    - [Clone Repository](#clone-repository)
    - [Update configuration](#update-configuration)
    - [Load Template](#load-template)
    - [Verify if it's loaded correctly](#verify-if-its-loaded-correctly)
  - [Deploy Bird](#deploy-bird)
  - [Additional actions](#additional-actions)
    - [show-protocols-all](#show-protocols-all)
    - [show-route](#show-route)
    - [show-status](#show-status)
  - [Stop, remove and clean up workloads and templates](#stop-remove-and-clean-up-workloads-and-templates)

## Prerequisites
- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

### Make sure monkd is running.

``` bash
foo@bar:~$ monk status
daemon: ready
auth: logged in
not connected to cluster
```

### Clone Repository

``` bash
git clone git@github.com:CuteAnonymousPanda/monk-bird.git
```

### Update configuration

This template has very basic Bird configuration and will start a running copy of Bird, however it is most desired that you would update it with the configuration you would need.
To do so edit the `manifest.yaml` and in the `files` section replace the config provided with yours.
You could also use a `contents: <<< bird.conf` notation to read the contents of the file instead of providing it in-line.

### Load Template

``` bash
cd monk-bird
monk load manifest.yaml
```

### Verify if it's loaded correctly

``` bash
$ monk list -l bird
✔ Got the list
Type      Template     Repository  Version  Tags
runnable  bird/server  local       -        -
```

## Deploy Bird

``` bash
$ monk run bird/server
✔ Starting the job: local/bird/server... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% ghcr.io/akafeng/bird:2.0.9 local
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ Started local/bird/server

🔩 templates/local/bird/server
 └─🧊 Peer local
    └─🔩 templates/local/bird/server
       └─📦 202f7b9e6ac73bb380e9de0464ca4593-local-bird-server-bird4
          ├─🧩 ghcr.io/akafeng/bird:2.0.9
          └─🔌 open tcp 1.1.1.1:179 -> 179

💡 You can inspect and manage your above stack with these commands:
        monk logs (-f) local/bird/server - Inspect logs
        monk shell     local/bird/server - Connect to the container's shell
        monk do        local/bird/server/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Additional actions

### show-protocols-all

``` bash
$ monk do bird/server/show-protocols-all
✔ Get templates/local/bird/server actions list success
✔ Got action parameters
✔ Parse parameters success
✔ Running action:
BIRD 2.0.9 ready.
Name       Proto      Table      State  Since         Info
device1    Device     ---        up     19:01:33.794

direct1    Direct     ---        down   19:01:33.794
  Channel ipv4
    State:          DOWN
    Table:          master4
    Preference:     240
    Input filter:   ACCEPT
    Output filter:  REJECT
  Channel ipv6
    State:          DOWN
    Table:          master6
    Preference:     240
    Input filter:   ACCEPT
    Output filter:  REJECT

kernel1    Kernel     master4    up     19:01:33.794
  Channel ipv4
    State:          UP
    Table:          master4
    Preference:     10
    Input filter:   ACCEPT
    Output filter:  ACCEPT
    Routes:         0 imported, 0 exported, 0 preferred
    Route change stats:     received   rejected   filtered    ignored   accepted
      Import updates:              0          0          0          0          0
      Import withdraws:            0          0        ---          0          0
      Export updates:              0          0          0        ---          0
      Export withdraws:            0        ---        ---        ---          0

kernel2    Kernel     master6    up     19:01:33.794
  Channel ipv6
    State:          UP
    Table:          master6
    Preference:     10
    Input filter:   ACCEPT
    Output filter:  ACCEPT
    Routes:         0 imported, 0 exported, 0 preferred
    Route change stats:     received   rejected   filtered    ignored   accepted
      Import updates:              0          0          0          0          0
      Import withdraws:            0          0        ---          0          0
      Export updates:              0          0          0        ---          0
      Export withdraws:            0        ---        ---        ---          0

static1    Static     master4    up     19:01:33.794
  Channel ipv4
    State:          UP
    Table:          master4
    Preference:     200
    Input filter:   ACCEPT
    Output filter:  REJECT
    Routes:         0 imported, 0 exported, 0 preferred
    Route change stats:     received   rejected   filtered    ignored   accepted
      Import updates:              0          0          0          0          0
      Import withdraws:            0          0        ---          0          0
      Export updates:              0          0          0        ---          0
      Export withdraws:            0        ---        ---        ---          0
✨ Took: 2s
```

### show-route

``` bash
$ monk do bird/server/show-route
✔ Get templates/local/bird/server actions list success
✔ Got action parameters
✔ Parse parameters success
✔ Running action:
BIRD 2.0.9 ready.
✨ Took: 2s
```

### show-status

``` bash
monk do bird/server/show-status
✔ Get templates/local/bird/server actions list success
✔ Got action parameters
✔ Parse parameters success
✔ Running action:
BIRD 2.0.9 ready.
BIRD 2.0.9
Router ID is 10.3.1.114
Hostname is fdddfa0c369c
Current server time is 2022-08-24 19:05:49.813
Last reboot on 2022-08-24 19:01:33.794
Last reconfiguration on 2022-08-24 19:01:33.794
Daemon is up and running
✨ Took: 2s
```

## Stop, remove and clean up workloads and templates

``` bash
monk purge    bird/server
monk purge -x bird/server
```

# docker.easyepg
A docker container for running easyepg with built-in cronjob

### Prerequisites
You will need to have `docker` installed on your system and the user you want to run it needs to be in the `docker` group.

> **Note:** The image is a multi-arch build providing variants for amd64, arm32v7 and arm64v8 - the correct variant for your architecture needs to be tagged e.g :amd64, :arm32v7, :arm64v8

## Technical info for docker GUIs (e.g. Synology, UnRaid, OpenMediaVault)
To learn how to manually start the container or about available parameters (you might need for your GUI used) see the following example:

```
docker run \
  -d \
  -e USER_ID="99" \
  -e GROUP_ID="100" \
  -e TIMEZONE="Europe/Berlin" \
  -e FREQUENCY="0 2 * * *" \
  -e UPDATE="yes" \
  -e REPO="sunsettrack4" \
  -e BRANCH="master" \
  -v {EASYEPG_STORAGE}:/easyepg \
  -v {XML_STORAGE}:/easyepg/xml \
  --name=easyepg \
  --restart unless-stopped \
  --tmpfs /tmp \
  --tmpfs /var/log \
  takealug/easyepg:tag
```

The available parameters in detail:

| Parameter | Optional | Values/Type | Default | Description |
| ---- | --- | --- | --- | --- |
| `USER_ID` | yes | [integer] | 99 | UID to run easyepg as |
| `GROUP_ID` | yes | [integer] | 100 | GID to run easyepg as |
| `TIMEZONE` | yes | [string] | Europe/Berlin | Timezone for the container |
| `FREQUENCY` | yes | [string] | 0 2 * * * | Cron frequency |
| `UPDATE` | yes | yes, no | yes | Turn On / Off easyepg Skript inside this Docker |
| `REPO` | yes | sunsettrack4, DeBaschdi | sunsettrack4 | The repo to update/install easyepg from |
| `BRANCH` | yes | [string] | master | The branch to update/install easyepg from |

Frequently used volumes:
 
| Volume | Optional | Description |
| ---- | --- | --- |
| `EASYEPG_STORAGE` | no | The directory to persist easyepg to |
| `XML_STORAGE` | yes | The directory to store the finished XML files in |

When passing volumes please replace the name including the surrounding curly brackets with existing absolute paths with correct permissions.

If you decide to remove `XML_STORAGE` the finished XML files can be found in the `xml` subdirectory of `EASYEPG_STORAGE` instead.

> **Note:** `XML_STORAGE` can, e.g., be used to directly write finished XMLs into the directory you pass into a separately running TVheadend docker container. 

## Crontab syntax
```
 ┌───────────── minute (0 - 59)
 │ ┌───────────── hour (0 - 23)
 │ │ ┌───────────── day of month (1 - 31)
 │ │ │ ┌───────────── month (1 - 12)
 │ │ │ │ ┌───────────── day of week (0 - 6) (Sunday to Saturday;
 │ │ │ │ │                                       7 is also Sunday on some systems)
 │ │ │ │ │
 │ │ │ │ │
 * * * * *  /command/to/execute
```

## First Run / Initial Setup Easyepg
Inside this Container you need to run /usr/local/bin/easyepg.process 
Connect to the Container e.g 
```
docker exec -ti easyepg /usr/local/bin/easyepg.process
```
> **Note:** For initial Setup instructions see :https://telerising.de/index.php/sample-page/easyepg/
>> Please, after you finish your Initial Setup, restart this Container to prevent Permission Problems.
>>> Now Easyepg wait for the next Cronjob and should create your XML

## Support my work
If you like my Work, please [![Paypal Donation Page](https://www.paypalobjects.com/en_US/i/btn/btn_donate_SM.gif)](https://paypal.me/DeBaschdi) - thank you! :-)

## Unraid Template
> **Note:** An Template for Unraid can be found here : https://raw.githubusercontent.com/DeBaschdi/docker.easyepg/master/Templates/Unraid/my-easyEPG.xml
> Please safe it to into \flash\config\plugins\dockerMan\templates-user, after that you can use this Template in Unraids Webui. Docker > Add Container > Select Template and choose easyEPG

<img src="https://raw.githubusercontent.com/DeBaschdi/docker.easyepg/master/Templates/Unraid/Screenshot.png" height="325" width="265">

# LXD Backup script

This Bash script backsup your LXD container (https://www.ubuntu.com/containers/lxd) with help of the powerful Minio Client (https://www.minio.io/features.html). 

## What does it do:

* This scripts creates a backup image from a snaphot from your LXC container that is managed with LXD. 
* The script will upload your image to a cloudstore with Minio Client (mc).
* Online backup of your LXC container
* Creates an easy and ready to use LXC image to import with the LXC import command. 

## What it does not cover:

* Create a constant database backup, this does not work with backups.
* Handle your data retention

# Installation instructions

You can run the script from the commandline, or place it in your cron. 

Prerequisites:

* Install Minio Client: https://minio.io/downloads.html#download-client
* Configure Minio Client by setting up your config hosts (mc config host add [minio host details])

Install it by cloning this repostory on your LXC host:

```
git clone https://github.com/systemlevel/lxdbackup.git
```

Then copy the lxdbackup to your $path:

```
cd lxdbackup && cp lxdbackup /usr/bin/
```

Make it executable:

```
chmod +x /usr/bin/lxdbackup
```

Then test it with:

```
lxdbackup container-name
```

## How do I set it automatically?

Use a cronjob like this to backup your container every day of the week on 01:10:

10 1 * * * flock -n /tmp/lxdbackup.lock lxdbackup container-name

## How do I restore a container?

Download your container image from your cloudstorage and import it with LXC or even import it directly if your cloudstorage  provides a HTTP(S) download for your image.

```
lxc image import <file> --alias <name>
```

Import from HTTPS directly:

```
lxc image import https://cloudrkt.com/lxdbackup --alias restored-image
```


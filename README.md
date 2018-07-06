# LXD Backup script

This small Bash script will backup your LXC container with help of the powerful Minio Client tools.

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

Install it by cloning this repostory on your LXC host:

```
git clone https://github.com/systemlevel/lxdbackup.git
```

Then copy the lxdbackup to your $path:

```
cd lxdbackup && cp lxdbackup /usr/bin/
```

Don't forget to make it executable:

```
chmod +x /usr/bin/lxbackup
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


[nc-scan-auto]: https://github.com/nextcloud/nextcloudpi/wiki/Configuration-Reference#nc-scan-auto
[nc-scan]: https://github.com/nextcloud/nextcloudpi/wiki/Configuration-Reference#nc-scan

[//]: Comment

<h3>

```diff
! TODO !
```
</h3>

- [ ] Update references in all the configuration entry files.


# Accessability


## NCP web interface : `nc-webui`
Enable or disable the WebUI.

#### How to enable
1. Change `ACTIVE` to `yes`.
2. Click `Run` or `Start` to apply your changes.


## SSH access : `SSH`

SSH can be enabled for an existing user. This is *pi* on Raspberry Pis or *root* on Armbian devices.
In order to enable SSH, the password can not remain set to the default.
You HAVE to create a NEW password for the user if you want this program to enable SSH, it will fail if you dont!
Note: Use normal AlphaNumeric, the only special characters allowed are .,@-_/

| Option    | Description |
|:----------|:------------|
| `ACTIVE`  | Indicates whether NCP can be accessed by SSH or not.
| `USER`    | User to grant SSH access to. It is automatically set to the root user of your device.
| `PASS`    | Password for above defined user. It must not be the default password.
| `CONFIRM` | Confirmation of `PASS`.

#### How to enable
1. Change `ACTIVE` to `yes`.
2. Define a password and confirm it.
3. Click `Run` or `Start` to apply your changes.


## nc-forward-ports
NextCloudPi has implemented a UPnP client to be able to configure the Router to port forward to your Raspberry Pi.

#### Requirements
You need to enable UPnP on your Router. Also disable it after you configure port forwarding.

#### How to configure
1. Navigate to `nc-forward-ports` in the TUI or the WebUI.
2. Set the ports your Nextcloud runs on. (It is recomended that you use the defaults)
3. Click Run or Start.



------
# Security


## Force secure HTTPS connections : `nc-httpsonly`
Force secure connection using HTTPS. This option is highly recommended.

#### How to enable
1. Change `ACTIVE` to `yes`.
2. Click `Run` or `Start` to apply your change.


## Firewall : `UFW`
Uncomplicated firewall that blocks traffic on all non-used ports.

| Option			| Description |
|:------------|:------------|
|`ACTIVE` | Enables or disables the UFW.
|`HTTP`   | Port for your HTTP traffic. Usually it is `80`.
|`HTTPS`  | Port for your HTTPS traffic. Usually it is `443`.
|`SSH`    | Port for your SSH traffic. Usually it is `22`.


## fail2ban
As soon as your NextClouPi is connected to the internet it might get attacked. Most attacks are probably automated attacks by botnets or scripts trying to break into your System by simply using standard username/password combinations like admin/admin. [fail2ban](https://github.com/fail2ban/fail2ban/wiki/How-fail2ban-works2) scans your webserver logs (which can be found under /var/log/apache2/error.log) for failed login attempts. If there are to many failed attempts (default is 6 failed attempts within 10 minutes) fail2ban will ban the attacker's IP address for a certain amount of time (default is 10 minutes). If you activate mail alerts you will receive emails when fail2ban locks out certain IP addresses. 
NextCloudPi uses fail2ban to secure Nextcloud logins as well as SSH logins.

| Option	  | Description |
|:------------|:------------|
|`ACTIVE`     | Has a tick if fail2ban is active
|`BANTIME` 		| Duration of a ban for a certain IP address after too many failed login attempts
|`FINDTIME` 	| Time slot (in seconds) in which failed login attempts are counted and the IP address gets banned
|`MAXRETRY` 	| Number of failed login attempts that trigger an IP address ban
|`MAILALERTS` | Activate/deactivate email notifications
|`EMAIL`			| Your personal email to receive ban notifications

#### Options
`BANTIME` : Duration of a ban for a certain IP address after too many failed login attempts

`FINDTIME` : Time slot in which failed login attempts are counted and the IP address gets banned

`MAXRETRY` : Number of failed login attempts that trigger an IP address ban

`MAILALERTS` : Activate/deactivate email notifications

`EMAIL` : Your personal email to receive ban notifications

#### How to activate
Run the TUI (`nextcloud-config`) or use the WebUI.
1. Change `ACTIVE` to `yes`
2. Change (optional) `BANTIME` (in seconds, default: 600 = 10 minutes) to change the duration of a ban for a certain IP address after too many failed login attempts.
3. Change (optional) `FINDTIME` (in seconds, default: 600 = 10 minutes) to change the time slot in which  failed login attempts are counted and the IP address gets banned.
4. Change (optional) `MAXRETRY` (default: 6 attempts) to change the number of failed login attempts that trigger an IP address ban.
5. Change (optional) `EMAIL` with your personal email to receive ban notifications.
6. Change (optional) `MAILALERTS` to activate/deactivate email notifications.
7. Click Run (WebUI) or Start (TUI)


## Certificates for secure connection : `letsencrypt`
In order to trust a connection to a website and send your user name and password, you need a SSL certificate. The SSL certificate ensures that the communication is encrypted, so everything you send can only be viewed by the server and not someone who impersonates him. By default NextCloudPi provides a self signed SSL certificate in order to encrypt your communication but it is strongly recomended that you use a certificate from a certificate authority. The NextCloudPi can run the Let's Encrypt client which gets a certificate from https://letsencrypt.org for your (sub)Domain Name. NextCloudPi also configures the web server to use it and renews the certificate once a month.

#### Configure
1. Navigate to `letsencrypt` in the TUI or the WebUI.
2. Change the `DOMAIN` with your (sub)Domain Name.
3. Change the `EMAIL` with your Email address. (It is recomended to use a valid Email address)
4. Click Run or Start.


## modsecurity
Web Application Firewall for extra security (experimental)

This is a really strong layer of security that makes sure that even if there is a vulnerability in Nextcloud code, it will be blocked by modsecurity in most cases.

The downside is that it can break some Apps, so disable it if something doesn't work for you. Tested with the most common Apps.

Learn more [here](https://ownyourbits.com/2017/03/23/modsecurity-web-application-firewall-for-nextcloud/)




# Automatic updates
It is recommended to keep both NextCloudPi and Nextcloud up to date all the time.
NextCloudPi can perform automatic updates.


## NextCloudPi : `nc-autoupdate-ncp`
Automatically update NextCloudPi.

#### How to enable
1. Navigate to `nc-autoupdate-ncp` in the TUI or the WebUI.
2. Change `ACTIVE` to `yes`.
3. Change the user to be notified when new updates are installed (default=admin).
4. Click Run or Start.


## Nextcloud : `unattended-upgrades`
Enable Automatic installation of security updates to keep your cloud safe.

#### How to enable
1. Navigate to `unattended-upgrades` in the TUI or the WebUI.
2. Change `ACTIVE` to `yes`.
3. Change `AUTOREBOOT` to `yes` if you want your Raspberry Pi to reboot automatically in order to apply updates (optional).
4. Click Run or Start.




---

# Dynamic DNS


Most home users do not have a static IP but rather a dynamic IP that changes from time to time. in order for you to be able to access your Nextcloud instance, from outside of your house, without typing an IP address you need a DDNS service which tracks IP changes and updates the DNS records. 
One possibility is to do this on your local router. Nevertheless, several providers are direcltly supported by NextCloudPi.

All the required data can be found in your account at the registered DDNS service provider.


## Free DNS : `DDNS_freeDNS`
[FreeDNS](https://freedns.afraid.org/) client.

You need to register an account on [FreeDNS](https://freedns.afraid.org/) and setup a (sub)Domain Name.

#### How to activate
Run the TUI (`nextcloud-config`) or use the WebUI.
Log in to freedns.afraid.com and click "Dynamic DNS". Right click on "Direct URL" next to your record. Paste it in a text editor and select only the hash (the letters after the "?").
1. Navigate to `freeDNS` in the TUI or the WebUI.
2. Change `ACTIVE` to `yes`
3. Change the `UPDATEHASH` with yours (delete the example and paste with ctrl+shift+V)
4. Change `DOMAIN` with your Domain Name you have registered.
5. (Optional) Change the `UPDATEINTERVAL` to the interval time you want the client to update your IP (Dynamic IPs do not change that often so you can leave the default (5mins)).
6. Click Run or Start.


## no-ip : `DDNS_no-ip`
Use the DDNS (Dynamic DNS) service by [No-IP](https://www.noip.com/).

Run the TUI (`nextcloud-config`) or use the WebUI.
1. Navigate to `no-ip` in the TUI or the WebUI.
2. Change `ACTIVE` to `yes`.
3. Change `USER` with your user name.
4. Change `PASS` with your password.
5. Change `DOMAIN` with your (sub)Domain Name.
6. Change `TIME` with the interval time you want to update the DNS record. Default 30mins.
7. Click Run or Start.


## Duck DNS : `DDNS_duckDNS`
[Duck DNS](https://www.duckdns.org/) is a free dynamic DNS hosted on Amazon VPC.




## spDYN : `DDNS_spDYN`
[spDYN](https://www.spdyn.de/) is a German dynamic DNS service provided by Securepoint GmbH.

When adding a host in your spDYN account, make sure that you choose an IPv4 host (type ´A´). You can enter just about any IP address, even a local one such as ´192.168.1.50´. This IP gets updated as soon as you activate spDYN in NextCloudPi.
Be aware that it may take more than half an hour until the DNS cache records in the internet are updated and your cloud is accessible using your new domain. During this time, even Let's Encrypt is not possible to activate.


## dnsmasq
This is a DNS server that you might need in case you cannot access your cloud from inside your house by the external URL, such as _mycloud.freeDNS.org_. This depends on wether your router supports _NAT loopback_. 

See [this post](https://ownyourbits.com/2017/03/09/dnsmasq-as-dns-cache-server-for-nextcloudpi-and-raspbian/) for details.




# Data structure and file system


## Automount USB drives : `nc-automount`
Enable this feature if you want your Rasperry Pi to automount USB drives.


## Format USB drive : `nc-format-USB`
Do this if you want to format your USB Drive and make it compatible with linux user/permissions system

> Make sure that **ONLY** the USB drive that you want to format is plugged in. 

> Be careful, this will destroy **ALL** data in the USB drive. Make sure that the Nextcloud database is not located on this drive!

>** YOU WILL LOSE ALL YOUR USB DATA **

#### How to run
1. Navigate to `nc-format-USB` in the TUI or the WebUI.
2. Change `LABEL` to a label you like.
3. Click Run or Start.


## Location of Nextcloud database : `nc-database`
Enable if you want to change the Nextcloud database location (e.g. to a usb drive).

> **Note** that non Unix filesystems such as NTFS are not supported because they do not provide a compatible user/permissions system.

>You need to use a USB drive that is permanently on and is responsive or the database will fail.     

> ** If it ever fails with a white page, move the database back to the SD **

The original location on the SD card is ...

#### How to configure
1. Navigate to `nc-database` in the TUI or the WebUI.
2. Change `DBDIR` to your database location.
3. Click Run or Start.


## Location of Nextcloud data : `nc-datadir`
Change the `data` folder location of Nextcloud.

> **Note** that non Unix filesystems such as NTFS are not supported because they do not provide a compatible user/permissions system

The original location on the SD card is ...

#### How to enable
1. Navigate to `nc-automount` in the TUI or the WebUI.
2. Change `ACTIVE` to yes.
3. Click Run or Start.


## NFS
Configure a NFS network file system server. This is a lightweight way to mount your cloud files through LAN in a Linux computer.

| Option   | Description |
|:---------|:------------|
| `ACTIVE` | Indicates whether NFS is active or not
| `DIR`    |
| `SUBNET` |
| `User`   |
| `GROUP`  |

Click `Run` or `Start` to apply your changes.


## samba
Configure SMB/CIFS file server (for Mac/Linux/Windows)

>If we intend to modify the data folder through SAMBA, then we have to synchronize NextCloud to make it aware of the changes. 
>This can be done manually or automatically using [`nc-scan`][nc-scan]  and [`nc-scan-auto`][nc-scan-auto] from `nextcloudpi-config`

#### How to configure
1. Navigate to `samba` in the TUI or the WebUI.
2. Change `ACTIVE` to `yes`.
3. Change `NCUSER` to your Nextcloud User (default=admin).
4. Change `USER` to the NextCloudPi User (default=pi).


## nc-swapfile
Change the location and the size of the swap file.

#### How to configure
1. Navigate to `nc-swapfile` in the TUI or the WebUI.
2. Change `SWAPFILE` to the location you want the new swap file to be.
3. Change `SWAPSIZE` to the desired size of the swap file (default=1024).
4. Click Run or Start.




# Maintenance and more


## Backup : `nc-backup`
Perform a manual backup.

#### How to configure
1. Navigate to `nc-backup` in the TUI or the WebUI.
2. Change `DESTDIR` to the desired location you want your backup to be.
3. Change `INCLUDEDATA` to `yes` if you want to include the Nextcloud data to the backup as well.
4. Change `BACKUPLIMIT` to the number of backups to be kept. If limit is reached, then the new backup will replace the older one.
5. Click Run or Start.


### Automated backups : `nc-backup-auto`

#### How to enable
1. Navigate to `nc-backup-auto` in the TUI or the WebUI.
2. Change `ACTIVE` to `yes`.
3. Change `DESTDIR` to a desired location for the backups.
4. Change `INCLUDEDATA` to `yes` (optional), to backup your data as well.
5. Change `BACKUPDAYS` to the number of days to perform the backup.
6. Change `BACKUPLIMIT` to the number of backups to be kept. If limit is reached, then the new backup will replace the older one.
7. Click Run or Start.


### Restore from backup : `nc-restore`


## nc-init
(Re)initiate Nextcloud to a clean configuration.


## nc-limits
Configure system limits for NextCloudPi.

> **Note** that `MAXFILESIZE` can be at maximum `2G` for now, due to limitation of 32bit php.

#### How to configure
1. Navigate to `nc-limits` in the TUI or the WebUI.
2. Change `MAXFILESIZE` to the desired maximum file size (<=2G).
3. Change `MEMORYLIMIT` to the memory limit you want (default=768M).
4. Click Run or Start.


## nc-nextcloud
Download and install a specific Nextcloud version. This destroys any existing instance. You need to run nc-init after completing nc-nextcloud, to take care of setting up database and cron jobs.


## nc-notify-updates
Get notified for updates (Pending or Installed) through the Nextcloud notification system.

#### How to enable
1. Navigate to `nc-notify-updates` in the TUI or the WebUI.
2. Change `ACTIVE` to `yes`.
3. Change `USER` to the user you want to be notified (default=admin).
4. Click Run or Start.


## nc-ramlogs
Enable mounting logs in RAM to prevent SD degradation (faster, consumes more RAM)

#### How to restore
1. Navigate to `nc-ramlogs` in the TUI or the WebUI.
2. Change `ACTIVE` to `yes`.
4. Click Run or Start.


## nc-scan
Perform a Nextcloud scan for user files.

#### How to run
1. Navigate to `nc-scan` in the TUI or the WebUI.


### nc-scan-auto
Automate a Nextcloud scan for user files.

#### How to enable
1. Navigate to `nc-scan-auto` in the TUI or the WebUI.
2. Change `ACTIVE` to `yes`.
3. Set `SCANINTERVAL` to the interval (in minutes) you want to scan every.
4. Click Run or Start.


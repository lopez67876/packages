<!-- markdownlint-disable -->

# banIP - ban incoming and outgoing IP addresses/subnets via sets in nftables

## Description
IP address blocking is commonly used to protect against brute force attacks, prevent disruptive or unauthorized address(es) from access or it can be used to restrict access to or from a particular geographic area — for example. Further more banIP scans the log file via logread and bans IP addresses that make too many password failures, e.g. via ssh.  

## Main Features
* banIP supports the following fully pre-configured domain blocklist feeds (free for private usage, for commercial use please check their individual licenses).  
  **Please note:** the columns "INP" and "FWD" show for which chains the feeds are suitable in common scenarios, e.g. the first entry should be limited to forward chain - see the config options 'ban\_blockforward' and 'ban\_blockinput' below.

| Feed                | Focus                          | INP | FWD | Information                                                           |
| :------------------ | :----------------------------: | :-: | :-: | :-------------------------------------------------------------------- |
| adaway              | adaway IPs                     |     |  x  | [Link](https://github.com/dibdot/banIP-IP-blocklists)                 |
| adguard             | adguard IPs                    |     |  x  | [Link](https://github.com/dibdot/banIP-IP-blocklists)                 |
| adguardtrackers     | adguardtracker IPs             |     |  x  | [Link](https://github.com/dibdot/banIP-IP-blocklists)                 |
| antipopads          | antipopads IPs                 |     |  x  | [Link](https://github.com/dibdot/banIP-IP-blocklists)                 |
| asn                 | ASN IPs                        |     |  x  | [Link](https://asn.ipinfo.app)                                        |
| backscatterer       | backscatterer IPs              |  x  |  x  | [Link](https://www.uceprotect.net/en/index.php)                       |
| bogon               | bogon prefixes                 |  x  |  x  | [Link](https://team-cymru.com)                                        |
| country             | country blocks                 |  x  |     | [Link](https://www.ipdeny.com/ipblocks)                               |
| cinsscore           | suspicious attacker IPs        |  x  |  x  | [Link](https://cinsscore.com/#list)                                   |
| darklist            | blocks suspicious attacker IPs |  x  |  x  | [Link](https://darklist.de)                                           |
| debl                | fail2ban IP blacklist          |  x  |  x  | [Link](https://www.blocklist.de)                                      |
| doh                 | public DoH-Provider            |     |  x  | [Link](https://github.com/dibdot/DoH-IP-blocklists)                   |
| drop                | spamhaus drop compilation      |  x  |  x  | [Link](https://www.spamhaus.org)                                      |
| dshield             | dshield IP blocklist           |  x  |  x  | [Link](https://www.dshield.org)                                       |
| edrop               | spamhaus edrop compilation     |  x  |  x  | [Link](https://www.spamhaus.org)                                      |
| feodo               | feodo tracker                  |  x  |  x  | [Link](https://feodotracker.abuse.ch)                                 |
| firehol1            | firehol level 1 compilation    |  x  |  x  | [Link](https://iplists.firehol.org/?ipset=firehol_level1)             |
| firehol2            | firehol level 2 compilation    |  x  |  x  | [Link](https://iplists.firehol.org/?ipset=firehol_level2)             |
| firehol3            | firehol level 3 compilation    |  x  |  x  | [Link](https://iplists.firehol.org/?ipset=firehol_level3)             |
| firehol4            | firehol level 4 compilation    |  x  |  x  | [Link](https://iplists.firehol.org/?ipset=firehol_level4)             |
| greensnow           | suspicious server IPs          |  x  |  x  | [Link](https://greensnow.co)                                          |
| iblockads           | Advertising IPs                |     |  x  | [Link](https://www.iblocklist.com)                                    |
| iblockspy           | Malicious spyware IPs          |  x  |  x  | [Link](https://www.iblocklist.com)                                    |
| myip                | real-time IP blocklist         |  x  |  x  | [Link](https://myip.ms)                                               |
| nixspam             | iX spam protection             |  x  |  x  | [Link](http://www.nixspam.org)                                        |
| oisdnsfw            | OISD-nsfw IPs                  |     |  x  | [Link](https://github.com/dibdot/banIP-IP-blocklists)                 |
| oisdsmall           | OISD-small IPs                 |     |  x  | [Link](https://github.com/dibdot/banIP-IP-blocklists)                 |
| proxy               | open proxies                   |  x  |     | [Link](https://iplists.firehol.org/?ipset=proxylists)                 |
| ssbl                | SSL botnet IPs                 |  x  |  x  | [Link](https://sslbl.abuse.ch)                                        |
| stevenblack         | stevenblack IPs                |     |  x  | [Link](https://github.com/dibdot/banIP-IP-blocklists)                 |
| talos               | talos IPs                      |  x  |  x  | [Link](https://talosintelligence.com/reputation_center)               |
| threat              | emerging threats               |  x  |  x  | [Link](https://rules.emergingthreats.net)                             |
| threatview          | malicious IPs                  |  x  |  x  | [Link](https://threatview.io)                                         |
| tor                 | tor exit nodes                 |  x  |     | [Link](https://github.com/SecOps-Institute/Tor-IP-Addresses)          |
| uceprotect1         | spam protection level 1        |  x  |  x  | [Link](http://www.uceprotect.net/en/index.php)                        |
| uceprotect2         | spam protection level 2        |  x  |  x  | [Link](http://www.uceprotect.net/en/index.php)                        |
| uceprotect3         | spam protection level 3        |  x  |  x  | [Link](http://www.uceprotect.net/en/index.php)                        |
| urlhaus             | urlhaus IDS IPs                |  x  |  x  | [Link](https://urlhaus.abuse.ch)                                      |
| urlvir              | malware related IPs            |  x  |  x  | [Link](https://iplists.firehol.org/?ipset=urlvir)                     |
| webclient           | malware related IPs            |  x  |  x  | [Link](https://iplists.firehol.org/?ipset=firehol_webclient)          |
| voip                | VoIP fraud blocklist           |  x  |  x  | [Link](https://voipbl.org)                                            |
| yoyo                | yoyo IPs                       |     |  x  | [Link](https://github.com/dibdot/banIP-IP-blocklists)                 |

* zero-conf like automatic installation & setup, usually no manual changes needed
* all sets are handled in a separate nft table/namespace 'banIP'
* full IPv4 and IPv6 support
* supports nft atomic set loading
* supports blocking by ASN numbers and by iso country codes
* supports local allow- and blocklist (IPv4, IPv6, CIDR notation or domain names)
* auto-add the uplink subnet to the local allowlist
* provides a small background log monitor to ban unsuccessful login attempts in real-time
* auto-add unsuccessful LuCI, nginx, Asterisk or ssh login attempts to the local blocklist
* fast feed processing as they are handled in parallel as background jobs
* per feed it can be defined whether the input chain or the forward chain should be blocked (default: both chains)
* automatic blocklist backup & restore, the backups will be used in case of download errors or during startup
* automatically selects one of the following download utilities with ssl support: aria2c, curl, uclient-fetch or wget
* supports a 'allowlist only' mode, this option restricts internet access from/to a small number of secure websites/IPs
* provides comprehensive runtime information
* provides a detailed set report
* provides a set search engine for certain IPs
* feed parsing by fast & flexible regex rulesets
* minimal status & error logging to syslog, enable debug logging to receive more output
* procd based init system support (start/stop/restart/reload/status/report/search)
* procd network interface trigger support
* ability to add new banIP feeds on your own

## Prerequisites
* **[OpenWrt](https://openwrt.org)**, latest stable release or a snapshot with nft/firewall 4 support  
* a download utility with SSL support: 'wget', 'uclient-fetch' with one of the 'libustream-*' SSL libraries, 'aria2c' or 'curl' is required
* a certificate store like 'ca-bundle', as banIP checks the validity of the SSL certificates of all download sites by default
* for E-Mail notifications you need to install and setup the additional 'msmtp' package

**Please note the following:**
* Devices with less than 256Mb of RAM are **_not_** supported
* Any previous installation of banIP must be uninstalled, and the /etc/banip folder and the /etc/config/banip configuration file must be deleted (they are recreated when this version is installed)
* There is no LuCI frontend at this time

## Installation & Usage
* update your local opkg repository (_opkg update_)
* install banIP (_opkg install banip_) - the banIP service is disabled by default
* edit the config file '/etc/config/banip' and enable the service (set ban\_enabled to '1'), then add pre-configured feeds via 'ban\_feed' (see the config options below)
* start the service with '/etc/init.d/banip start' and check check everything is working by running '/etc/init.d/banip status'

## banIP CLI interface
* All important banIP functions are accessible via CLI. A LuCI frontend will be available in due course.
```
~# /etc/init.d/banip
Syntax: /etc/init.d/banip [command]

Available commands:
	start           Start the service
	stop            Stop the service
	restart         Restart the service
	reload          Reload configuration files (or restart if service does not implement reload)
	enable          Enable service autostart
	disable         Disable service autostart
	enabled         Check if service is started on boot
	report          [text|json|mail] Print banIP related set statistics
	search          [<IPv4 address>|<IPv6 address>] Check if an element exists in the banIP sets
	running         Check if service is running
	status          Service status
	trace           Start with syscall trace
	info            Dump procd service info
```

## banIP config options

| Option                  | Type   | Default                       | Description                                                                           |
| :---------------------- | :----- | :---------------------------- | :------------------------------------------------------------------------------------ |
| ban_enabled             | option | 0                             | enable the banIP service                                                              |
| ban_nicelimit           | option | 0                             | ulimit nice level of the banIP service (range 0-19)                                   |
| ban_filelimit           | option | 1024                          | ulimit max open/number of files (range 1024-4096)                                     |
| ban_loglimit            | option | 100                           | the logread monitor scans only the last n lines of the logfile                        |
| ban_logcount            | option | 1                             | how many times the IP must appear in the log to be considered as suspicious           |
| ban_logterm             | list   | regex                         | various regex for logfile parsing (default: dropbear, sshd, luci, nginx, asterisk)    |
| ban_autodetect          | option | 1                             | auto-detect wan interfaces, devices and subnets                                       |
| ban_debug               | option | 0                             | enable banIP related debug logging                                                    |
| ban_loginput            | option | 1                             | log drops in the input chain                                                          |
| ban_logforward          | option | 0                             | log rejects in the forward chain                                                      |
| ban_autoallowlist       | option | 1                             | add wan IPs/subnets automatically to the local allowlist                              |
| ban_autoblocklist       | option | 1                             | add suspicious attacker IPs automatically to the local blocklist                      |
| ban_allowlistonly       | option | 0                             | restrict the internet access from/to a small number of secure websites/IPs            |
| ban_reportdir           | option | /tmp/banIP-report             | directory where banIP stores the report files                                         |
| ban_backupdir           | option | /tmp/banIP-backup             | directory where banIP stores the compressed backup files                              |
| ban_protov4             | option | - / autodetect                | enable IPv4 support                                                                   |
| ban_protov6             | option | - / autodetect                | enable IPv4 support                                                                   |
| ban_ifv4                | list   | - / autodetect                | logical wan IPv4 interfaces, e.g. 'wan'                                               |
| ban_ifv6                | list   | - / autodetect                | logical wan IPv6 interfaces, e.g. 'wan6'                                              |
| ban_dev                 | list   | - / autodetect                | wan device(s), e.g. 'eth2'                                                            |
| ban_trigger             | list   | -                             | logical startup trigger interface(s), e.g. 'wan'                                      |
| ban_triggerdelay        | option | 10                            | trigger timeout before banIP processing begins                                        |
| ban_deduplicate         | option | 1                             | deduplicate IP addresses across all active sets                                       |
| ban_splitsize           | option | 0                             | split ext. sets after every n lines/members (saves RAM)                               |
| ban_cores               | option | - / autodetect                | limit the cpu cores used by banIP (saves RAM)                                         |
| ban_nftexpiry           | option | -                             | expiry time for auto added blocklist members, e.g. '5m', '2h' or '1d'                 |
| ban_nftpriority         | option | -200                          | nft banIP table priority (default is the prerouting table priority)                   |
| ban_feed                | list   | -                             | external download feeds, e.g. 'yoyo', 'doh', 'country' or 'talos' (see feed table)    |
| ban_asn                 | list   | -                             | ASNs for the 'asn' feed, e.g.'32934'                                                  |
| ban_country             | list   | -                             | country iso codes for the 'country' feed, e.g. 'ru'                                   |
| ban_blockinput          | list   | -                             | limit a feed to the input chain, e.g. 'country'                                       |
| ban_blockforward        | list   | -                             | limit a feed to the forward chain, e.g. 'doh'                                         |
| ban_fetchcmd            | option | - / autodetect                | 'uclient-fetch', 'wget', 'curl' or 'aria2c'                                           |
| ban_fetchparm           | option | - / autodetect                | set the config options for the selected download utility                              |
| ban_fetchinsecure       | option | 0                             | don't check SSL server certificates during download                                   |
| ban_mailreceiver        | option | -                             | receiver address for banIP related notification E-Mails                               |
| ban_mailsender          | option | no-reply@banIP                | sender address for banIP related notification E-Mails                                 |
| ban_mailtopic           | option | banIP notification            | topic for banIP related notification E-Mails                                          |
| ban_mailprofile         | option | ban_notify                    | mail profile used in 'msmtp' for banIP related notification E-Mails                   |
| ban_resolver            | option | -                             | external resolver used for DNS lookups                                                |
| ban_feedarchive         | option | /etc/banip/banip.feeds.gz     | full path to the compressed feed archive file used by banIP                           |

## Examples
**banIP report information**  
```
~# /etc/init.d/banip report
:::
::: banIP Set Statistics
:::
    Timestamp: 2023-02-08 22:12:40
    ------------------------------
    auto-added to allowlist: 1
    auto-added to blocklist: 0

    Set                  | Set Elements  | Chain Input   | Chain Forward | Input Packets | Forward Packets
    ---------------------+---------------+---------------+---------------+---------------+----------------
    allowlistvMAC        | 0             | n/a           | OK            | n/a           | 0             
    allowlistv4          | 1             | OK            | OK            | 0             | 0             
    allowlistv6          | 0             | OK            | OK            | 0             | 0             
    blocklistvMAC        | 0             | n/a           | OK            | n/a           | 0             
    blocklistv4          | 0             | OK            | OK            | 0             | 0             
    blocklistv6          | 0             | OK            | OK            | 0             | 0             
    dohv4                | 542           | n/a           | OK            | n/a           | 22            
    adguardv4            | 23007         | n/a           | OK            | n/a           | 18            
    yoyov4               | 1936          | n/a           | OK            | n/a           | 1             
    oisdbasicv4          | 26000         | n/a           | OK            | n/a           | 325           
    ---------------------+---------------+---------------+---------------+---------------+----------------
    10                   | 51486         | 4             | 10            | 0             | 366
```

**banIP runtime information**  
```
~# etc/init.d/banip status
::: banIP runtime information
  + status            : active
  + version           : 0.8.0
  + element_count     : 51486
  + active_feeds      : allowlistvMAC, allowlistv4, allowlistv6, blocklistvMAC, blocklistv4, blocklistv6, dohv4, adguardv4
                        , yoyov4, oisdbasicv4
  + active_devices    : eth2
  + active_interfaces : wan
  + active_subnets    : 192.168.98.107/24
  + run_info          : base_dir: /tmp, backup_dir: /tmp/banIP-backup, report_dir: /tmp/banIP-report, feed_archive: /etc/b
                        anip/banip.feeds.gz
  + run_flags         : protocol (4/6): ✔/✘, log (inp/fwd): ✔/✘, deduplicate: ✔, split: ✘, allowed only: ✘
  + last_run          : action: start, duration: 0m 15s, date: 2023-02-08 22:12:46
  + system_info       : cores: 2, memory: 3614, device: PC Engines apu1, OpenWrt SNAPSHOT r21997-b5193291bd
```

**banIP search information**  
```
~# /etc/init.d/banip search 221.228.105.173
:::
::: banIP Search
:::
    Looking for IP 221.228.105.173 on 2023-02-08 22:12:48
    ---
    IP found in set oisdbasicv4
```

**allow-/blocklist handling**  
banIP supports local allow and block lists (IPv4, IPv6, CIDR notation or domain names), located in /etc/banip/banip.allowlist and /etc/banip/banip.blocklist.  
Unsuccessful login attempts or suspicious requests will be tracked and added to the local blocklist (see the 'ban\_autoblocklist' option). The blocklist behaviour can be further tweaked with the 'ban\_nftexpiry' option.  
Furthermore the uplink subnet will be added to local allowlist (see 'ban\_autowallowlist' option).  
Both lists also accept domain names as input to allow IP filtering based on these names. The corresponding IPs (IPv4 & IPv6) will be extracted in a detached background process and added to the sets.

**allowlist-only mode**  
banIP supports an "allowlist only" mode. This option restricts the internet access from/to a small number of secure websites/IPs, and block access from/to the rest of the internet. All IPs and Domains which are _not_ listed in the allowlist are blocked.

**redirect Asterisk security logs to lodg/logread**   
banIP only supports logfile scanning via logread, so to monitor attacks on Asterisk, its security log must be available via logread. To do this, edit '/etc/asterisk/logger.conf' and add the line 'syslog.local0 = security', then run 'asterisk -rx reload logger' to update the running Asterisk configuration.

**tweaks for low memory systems**  
nftables supports the atomic loading of rules/sets/members, which is cool but unfortunately is also very memory intensive. To reduce the memory pressure on low memory systems (i.e. those with 256-512Mb RAM), you should optimize your configuration with the following options:  

    * point 'ban_reportdir' and 'ban_backupdir' to an external usb drive
    * set 'ban_cores' to '1' (only useful on a multicore system) to force sequential feed processing
    * set 'ban_splitsize' e.g. to '1000' to split the load of an external set after every 1000 lines/members

**tweak the download options**  
By default banIP uses the following pre-configured download options:
```
    * aria2c: --timeout=20 --allow-overwrite=true --auto-file-renaming=false --log-level=warn --dir=/ -o
    * curl: --connect-timeout 20 --silent --show-error --location -o
    * uclient-fetch: --timeout=20 -O
    * wget: --no-cache --no-cookies --max-redirect=0 --timeout=20 -O
```
To override the default set 'ban_fetchparm' manually to your needs.

**send E-Mail notifications via 'msmtp'**  
To use the email notification you must install & configure the package 'msmtp'.  
Modify the file '/etc/msmtprc', e.g.:
```
[...]
defaults
auth            on
tls             on
tls_certcheck   off
timeout         5
syslog          LOG_MAIL
[...]
account         ban_notify
host            smtp.gmail.com
port            587
from            <address>@gmail.com
user            <gmail-user>
password        <password>
```
Finally add a valid E-Mail receiver address.

**add new banIP feeds**  
The banIP blocklist feeds are stored in an external, compressed JSON file '/etc/banip/banip.feeds.gz'.  
To add a new or edit an existing feed extract the compressed JSON file _gunzip /etc/banip/banip.feeds.gz_.
A valid JSON source object contains the following required information, e.g.:
```
	[...]
	"tor": {
		"url_4": "https://raw.githubusercontent.com/SecOps-Institute/Tor-IP-Addresses/master/tor-exit-nodes.lst",
		"url_6": "https://raw.githubusercontent.com/SecOps-Institute/Tor-IP-Addresses/master/tor-exit-nodes.lst",
		"rule_4": "/^(([0-9]{1,3}\\.){3}(1?[0-9][0-9]?|2[0-4][0-9]|25[0-5])(\\/(1?[0-9]|2?[0-9]|3?[0-2]))?)$/{printf \"%s,\\n\",$1}",
		"rule_6": "/^(([0-9A-f]{0,4}:){1,7}[0-9A-f]{0,4}:?(\\/(1?[0-2][0-8]|[0-9][0-9]))?)$/{printf \"%s,\\n\",$1}",
		"focus": "tor exit nodes",
		"descurl": "https://github.com/SecOps-Institute/Tor-IP-Addresses"
	},
	[...]
```
Add an unique object name, make the required changes and compress the changed JSON file finally with _gzip /etc/banip/banip.feeds_ to use the new feed file in banIP.  
**Please note:** if you're going to add new feeds, **always** work with a copy of the default file; this file is always overwritten with every banIP update. To reference your own file set the option 'ban\_feedarchive' accordingly

## Support
Please join the banIP discussion in this [forum thread](https://forum.openwrt.org/t/banip-support-thread/16985) or contact me by mail <dev@brenken.org>

## Removal
* stop all banIP related services with _/etc/init.d/banip stop_
* optional: remove the banip package (_opkg remove banip_)

Have fun!  
Dirk

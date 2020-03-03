systemd-timesyncd
=================

[![Build Status](https://api.travis-ci.com/hafu/ansible-role-systemd-timesyncd.svg?branch=master)](https://travis-ci.com/hafu/ansible-role-systemd-timesyncd)

Configures *systemd-timesyncd* daemon. Optionally removes other ntp daemons 
like *chrony*, *ntp* or *openntpd* and/or set the timezone.

Requirements
------------

Systemd with timesyncd.

Role Variables
--------------

Default variables are listed in `defaults/main.yml`, OS depended variables are
listed in `vars/`.

```yaml
systemd_timesycd_remove_other_daemons: false
systemd_timesycd_other_daemons: []
```

No other NTP daemons will be removed by default. To remove may conflicting 
daemons set `systemd_timesycd_remove_other_daemons` to `true`. The list of
packages to remove is set in the playbook via the provided YAML file in `vars`
directory.

```yaml
systemd_timesycd_set_timezone: false
systemd_timesycd_timezone: Etc/UTC
```

Setting the timezone is also by default deactivated. To set a timezone set
`systemd_timesycd_set_timezone` to `true` and provide a timezone 
`systemd_timesycd_timezone`.

```yaml
systemd_timesycd_conf_ntp_servers: []
systemd_timesycd_conf_fallback_ntp:
  - 0.pool.ntp.org
  - 1.pool.ntp.org
  - 2.pool.ntp.org
  - 3.pool.ntp.org
```

The variable `systemd_timesycd_conf_ntp_servers` should provide a list of NTP
serves. This sets the option `NTP=` of 
[timesyncd.conf](https://www.freedesktop.org/software/systemd/man/timesyncd.conf.html)
The service *systemd-networkd* may combines this list with servers acquired by
*systemd-networkd*.

Servers in `systemd_timesycd_conf_fallback_ntp` will be used when no servers
are provided by `systemd_timesycd_conf_ntp_servers` and *systemd-networkd*
also can not obtain any NTP server. This is the equivalent to `FallbackNTP=` of
[timesyncd.conf](https://www.freedesktop.org/software/systemd/man/timesyncd.conf.html).

```yaml
systemd_timesycd_conf_root_distance_max_sec: 5
systemd_timesycd_conf_poll_interval_min_sec: 32
systemd_timesycd_conf_poll_interval_max_sec: 2048
```

These are optional and can be set if desired:
*  `systemd_timesycd_conf_root_distance_max_sec` -> `RootDistanceMaxSec`
*  `systemd_timesycd_conf_poll_interval_min_sec` -> `PollIntervalMinSec`
*  `systemd_timesycd_conf_poll_interval_max_sec` -> `PollIntervalMaxSec`

See [timesyncd.conf](https://www.freedesktop.org/software/systemd/man/timesyncd.conf.html) 

Dependencies
------------

None

Example Playbook
----------------

This example removes other installed NTP daemons, sets the timezone to
`Europe/Berlin` and uses german pool NTP servers. 

```yaml
- hosts: servers
  roles:
     - role: hafu.systemd-timesyncd
       systemd_timesycd_remove_other_daemons: true
       systemd_timesycd_set_timezone: true
       systemd_timesycd_timezone: Europe/Berlin
       systemd_timesycd_conf_fallback_ntp:
         - 0.de.pool.ntp.org
         - 1.de.pool.ntp.org
         - 2.de.pool.ntp.org
         - 3.de.pool.ntp.org
```

License
-------

MIT

Author Information
------------------

https://github.com/hafu
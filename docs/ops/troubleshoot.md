# Troubleshooting tips

## Get core file
Common settings on the system:
* set core file size to unlimited: `ulimit -c unlimited`

### Core from unexpected process quit

First you need to activate xrootd to create core file when process exits unexpectedly:

* set environment variable upon cmsd/xrootd start `DAEMON_COREFILE_LIMIT="unlimited"`
  * RHEL6: variable set in `/etc/sysconfig/xrootd` or set as root `ulimit -c unlimited`
  * RHEL7: you can use `/etc/sysconfig/xrootd` but prepare override file `/etc/systemd/system/cmsd@clustered.service.d/override.conf` with content:
```
     [Service]
     EnvironmentFile=-/etc/sysconfig/xrootd
```

### Core from running process
* directly if you know PID: 
```
$ gdb --pid=PID_NR
(gdb) gcore
```

* if you know process name (e.g. xrootd):
```
`$ gcore $(pidof xrootd)`
```

### Core from not running process:
* by default, XRootD should leave `core file` in the `/var/spool/xrootd/` when it crashes

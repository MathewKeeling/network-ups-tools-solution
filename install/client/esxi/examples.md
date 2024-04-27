# ESXI Connecting to NUT


## Example of a successful Connection to NUT from ESXi

```log
2024-01-01T00:00:00.000Z smartd[REDACTED]: [warn] <redacted>: REALLOCATED SECTOR CT below threshold (0 < 90)
2024-01-01T00:00:00.000Z crond[REDACTED]: USER root pid 00000 cmd /bin/hostd-probe.sh ++group=host/vim/vmvisor/hostd-probe/stats/sh
2024-01-01T00:00:00.000Z NUT[REDACTED]: NUT client is running
2024-01-01T00:00:00.000Z upsmon[REDACTED]: Signal 15: exiting
2024-01-01T00:00:00.000Z NUT[REDACTED]: NUT client stopped
2024-01-01T00:00:00.000Z upsmon[REDACTED]: Startup successful
2024-01-01T00:00:00.000Z upsmon[REDACTED]: Warning: running as one big root process by request (upsmon -p)
2024-01-01T00:00:00.000Z upsmon[REDACTED]: upsnotify: failed to notify about state 2: no notification tech defined, will not spam more about it
2024-01-01T00:00:00.000Z NUT[REDACTED]: NUT client started
2024-01-01T00:00:00.000Z NUT[REDACTED]: NUT client is running
2024-01-01T00:00:00.000Z sfcbd-init[REDACTED]: args ('status')
2024-01-01T00:00:00.000Z sfcbd-init[REDACTED]: Getting Exclusive access, please wait...
2024-01-01T00:00:00.000Z sfcbd-init[REDACTED]: Exclusive access granted.
2024-01-01T00:00:00.000Z upsmon[REDACTED]: UPS <REDACTED>@<0.0.0.0> on battery
```
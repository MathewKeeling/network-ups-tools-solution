# NUT: Logs


## Validate Connections

```bash
tail -f  /var/log/syslog

# example output:
Apr 27 00:57:54 <REDACTED> upsd[14769]: User <REDACTED>@<REDACTED> logged into UPS [<REDACTED>]
Apr 27 00:59:01 <REDACTED> upsd[14769]: User <REDACTED>@<REDACTED> logged out from UPS [<REDACTED>]
Apr 27 00:59:41 <REDACTED> upsd[14769]: User <REDACTED>@<REDACTED> logged into UPS [<REDACTED>]
Apr 27 01:05:00 <REDACTED> upsd[14769]: User <REDACTED>@<REDACTED> logged out from UPS [<REDACTED>]
Apr 27 01:05:01 <REDACTED> upsd[14769]: User <REDACTED>@<REDACTED> logged into UPS [<REDACTED>]
```
# NUT: Event Configuraiton


## Overview

At this time, devices are configured to shutdown after 60 seconds of power loss. No attention is given to the percentage of the battery remaining.

It is understood that if power is out for longer than 60 seconds--there is probably an event with a high severity and therefore the safety of the systems is prioritized.


### Exceptions

Unless you use SSH to configure the upsd daemon on the Synology, the only configuration supported is SNMP. You can choose to do an shutdown after x seconds, or shutdown at a certain battery percentage.


## Low Battery Event

```
tbd
```


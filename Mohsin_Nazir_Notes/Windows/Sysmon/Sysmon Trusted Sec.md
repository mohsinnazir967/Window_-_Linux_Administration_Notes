# Sysmon Introduction 

- [Sysmon TrustedSec](https://github.com/trustedsec/SysmonCommunityGuide/tree/master) GitHub documentation.
- [Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) is a system monitoring tool.

## Installation

- Download from sysinternals website. 
- Unzip and install the using the powershell as administrator

*install using the Powershell* 

```
sysmon.exe -i -accepteula
```

## Configuration

**Installing VS Code Sysmon extension**

- Find Sysmon extension 
- Install it.

**Installing PowerShell module PSGumshoe**

 install using powershell
 
```
install-module -name psgumshoe
```

**Configurations** 

*To view the configuration of sysmon.*

```
sysmon -c
```

 *Resetting configuration with defaults*

```
sysmon -c --
```

*Event 16 created for every time a change took place inside sysmon. Getting event 16.*

```
get-sysmonconfigchange | more
```

*Enables Hashing algorithms* 

```
sysmon -c -h **
```

*Printing the schema*

```
sysmon -s
```

*Sysmon should log the loading of the specified executable file.*

```
sysmon -c -l malicious.exe
```

*Track network connections for specified process/processes.*

```
sysmon -c -n malicious.axe
```

*Track anybody trying to access a specific process*

```
sysmon -c -k lsass.exe
``` 

Does not work on newer edition, confirm by using

```
get-SysmonProcessAccess
```

----------------
## Rules & Filter Order

- Filter triggers in the order in which they are in schema

- If rules are above the filers then rules get triggered

- If rules are below the trigger then the filters are triggred.



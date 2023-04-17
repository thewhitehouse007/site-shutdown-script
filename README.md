# shutdown-site.slax

************************************************
This script is an op script that will shutdown both the switch and router, on a site. 
Naming convention is important as XXXX--FW01 = sitename is used to lookup the switch DNS IP and connect
using hostname XXXX-SW01
***************************************************
To run this script it must be installed in
 **/var/db/scripts/op/**  on the Firewall.
 
The following configuration must be present:
```
set system scripts op file shutdown-site.slax
```

Type: `op shutdown-site` to run it
***************************************************
This command can also be executed by a specific login user by setting a user class with a login script. The script will run imediately once the user has logged in, preventing the user from accessing the device via CLI. 
```
top edit system login class SHUTDOWN 
set login-script shutdown-site.slax
set permissions maintenance
set allow-commands "{request system power-off}"
top edit system login user shutdown-user
set class SHUTDOWN
set authentication plain-text-password
commit
```
NOTE: This user class must be set on both Firewall and Switches, however, do not configure the login script on the switch.
Alternatively, just setup the user class and user name on all devices and ask the user to run `op shutdown_site` manually.
*************************************************
## Script Output

### Aborted

```
XXXX-FW01 (ttyu0)

login: shutdown-user
Password:

--- JUNOS 20.2R3-S2.5 built 2021-07-30 09:45:37 UTC

* ************************************************************************* *
*                                                                           *
*         = WARNING = SITE SHUTDOWN SCRIPT IS ABOUT TO BE EXECUTED =        *
*                                                                           *
*  This script is intended to shut-down & power-off both Router and Switch  *
*  at this site. This operation is irreversible and power-on will require   *
*  on-site intervention.                                                    *
*                                                                           *
* ************************************************************************* *


Are you sure you want to execute this operation? (yes/no) no

* ************************************************************************* *
*                  = EXITTING SHUTDOWN SCRIPT IMMEDIATELY =                 *
*                                 Good-bye!                                 *
* ************************************************************************* *

XXXX-FW01 (ttyu0)
login:
```

### Confirmed
```
login: shutdown-user
Password:

--- JUNOS 20.2R3-S2.5 built 2021-07-30 09:45:37 UTC

* ************************************************************************* *
*                                                                           *
*         = WARNING = SITE SHUTDOWN SCRIPT IS ABOUT TO BE EXECUTED =        *
*                                                                           *
*  This script is intended to shut-down & power-off both Router and Switch  *
*  at this site. This operation is irreversible and power-on will require   *
*  on-site intervention.                                                    *
*                                                                           *
* ************************************************************************* *


Are you sure you want to execute this operation? (yes/no) yes
The authenticity of host 'xxxx-sw01 (x.x.8.51)' can't be established.
ECDSA key fingerprint is SHA256:r8mgY7AXX7YksS8+yqNhJ4MJBtDhquhBJXOXlqsa868.
Are you sure you want to continue connecting (yes/no)? yes
shutdown-user@xxxx-sw01's password:
SWITCH XXXX-SW01 SHUTTING DOWN & POWERING OFF: Stand-by!
ROUTER XXXX-FW01 SHUTTING DOWN & POWERING OFF: Good-bye!

*** FINAL System shutdown message from shutdown-user@XXXX-FW01 ***

System going down IMMEDIATELY

shutdown_user@XXXX-FW01> MMWaiting (max 60 seconds) for system process `vnlru' to stop...done
Waiting (max 60 seconds) for system process `vnlru_mem' to stop...done
Waiting (max 60 seconds) for system process `bufdaemon' to stop...done
Waiting (max 60 seconds) for system process `syncer' to stop...
Syncing disks, vnodes remaining...0 0 0 0 0 done

syncing disks... All buffers synced.
Uptime: 1h58m11s
The operating system has halted.
Turning the system power off.

The operating system has halted.

Session stopped
```

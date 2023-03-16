# shutdown_site.slax

************************************************
This script is an op script that will shutdown 
both the switch and router, on a site. 
Naming convention is important as XXXX--FW01 = sitename
is used to lookup the switch DNS IP and connect
using hostname XXXX-SW01
***************************************************
To run this script it must be installed in
 **/var/db/scripts/op/**  on the Firewall.
 
The following configuration must be present:
```
set system scripts op file shutdown_site.slax
```

Type: `op shutdown_site` to run it
***************************************************
This command can also be executed by a specific login user by setting a user class with a login script. The script will run imediately once the user has logged in, preventing the user from accessing the device via CLI. 
```
top edit system login class SHUTDOWN 
set login-script shutdown_site.slax
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

```
XXXX-FW01 (ttyu0)

login: shutdown_user
Password:

--- JUNOS 15.1X49-D150.2 built 2018-09-19 17:44:55 UTC

* ****************************************************************************
*
*               - WARNING - SHUTDOWN SCRIPT EXECUTED -
*
*  This script is intented to shutdown both Router and Switch at this
*  site. This operation will be irreversable and power-on will require
*  on-site hands.
*
* ****************************************************************************
Are you sure you want to execute this operation? (yes/no)
Are you sure you want to execute this operation? (yes/no) yes
The authenticity of host 'XXX-sw01 (xx.xx.xx.xx)' can't be established.
ECDSA key fingerprint is SHA256:r8mgY7xxxxxYksS8+yqNhJxxxxxDhquhBxxxxxsa8xxx.
Are you sure you want to continue connecting (yes/no)? yes
shutdown_user@xxxx-sw01's password:
shutdown_user@xxxx-sw01's password:

This command will halt all the members.
If planning to halt only one member use the member option
SWITCH XXXX-SW01 IS GOING SHUTDOWN: Good-Bye!
ROUTER XXXX-FW01 IS GOING SHUTDOWN: Good-Bye!


*** FINAL System shutdown message from shutdown_user@XXXX-FW01 ***

System going down IMMEDIATELY


{primary:node0}
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

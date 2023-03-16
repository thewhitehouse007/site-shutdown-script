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

Type: **op shutdown_site** to run it
***************************************************
This command can also be executed by a specific login user by setting a user class with a login script 
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

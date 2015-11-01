# Adding an host in the monitoring

## Declaring a new host

```
   # On the main Shinken server
   su - shinken

   # Shinken hosts declaration are here ...
   cd /etc/shinken/hosts

   # Hosts located on primary site (centro de distribução) are here ...
   cd main

   # Hosts located on secondary site (centrinho) are here ...
   # cd second

   # Copy an host file sample
   # - remote-poller.cfg, is a sample for a linux SNMP monitored host
   # - uvb-pm.cfg, is a sample for a linux NRPE monitored host
   # - img-srv-021.cfg, is a sample for a Windows WMI monitored host

   # -------------------------------------
   # Windows sample
   cp img-srv-021.cfg srv-dom01

   # Edit the new file
   vi srv-dom01.cfg

   # Set the host content
   # - in the use property, set smbits-main for hosts located in main center, or smbits-second for hosts in secondary center ...

   define host{
      use                     generic-host, windows, important, smbits-main
      contact_groups          admins
      host_name               srv-dom01
      alias                   Windows Server / Backup Server / NAS
      display_name            srv-dom01
      address                 192.168.168.8
   }
   # -------------------------------------

   # -------------------------------------
   # Linux SNMP sample
   cp remote-poller.cfg img-srv-0001.cfg

   # Edit the new file
   vi img-srv-0001.cfg

   # Set the host content
   # - in the use property, set smbits-main for hosts located in main center, or smbits-second for hosts in secondary center ...

   define host{
      use                     generic-host, linux-snmp, top-for-business, smbits-main
      contact_groups          admins
      host_name               img-srv-001
      alias                   Shinken remote poller
      display_name            img-srv-001
      address                 192.168.168.13
   }
   # -------------------------------------

   # -------------------------------------
   # Linux NRPE sample
   cp uvb-pm.cfg uvb-xen01-1.cfg

   # Edit the new file
   vi uvb-xen01-1.cfg

   # Set the host content
   # - in the use property, set smbits-main for hosts located in main center, or smbits-second for hosts in secondary center ...

   define host{
      use                     generic-host, linux-nrpe, important, smbits-main
      contact_groups          admins
      host_name               uvb-xen01-1
      alias                   Unitrends Backup / Presentation
      display_name            uvb-xen01-1
      address                 192.168.168.249
   }
   # -------------------------------------

   # Save the file

   # Check shinken configuration (there must be no errors)
   shinken-arbiter -v -c /etc/shinken/shinken.cfg

   # Last lines must be like this ... note hosts number to 14!
   [1446301825] INFO: [Shinken] Number of hosts in the realm All: 14 (distributed in 9 linked packs)
   [1446301825] INFO: [Shinken] Total number of hosts : 14
   [1446301825] INFO: [Shinken] Things look okay - No serious problems were detected during the pre-flight check

   # If errors are signaled, edit the file and fix the reported errors.

   # Restart Shinken
   su -
   root@shinken:~# /etc/init.d/shinken restart
   Restarting scheduler
   . ok
   Restarting poller
   . ok
   Restarting reactionner
   . ok
   Restarting broker
   . ok
   Restarting receiver
   . ok
   Restarting arbiter
   Doing config check
   . ok
   . ok

   # All must be ok, the new host is now monitored

```
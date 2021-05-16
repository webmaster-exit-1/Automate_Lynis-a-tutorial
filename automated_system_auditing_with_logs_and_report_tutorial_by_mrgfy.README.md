## Want to automate auditing your system's security with relative ease? 
## Follow these simple steps, and you good to go.

__Step 1.__
Install `lynis`

__Step 2.__
Create service unit (/etc/systemd/system/lynis.service)

```
#################################################################################
#
# Lynis service file for systemd
#
#################################################################################
# Do not remove, so Lynis can provide a hint when a newer unit is available
# Generator=lynis
# Version=1
#################################################################################

[Unit]
Description=Security audit and vulnerability scanner
Documentation=https://cisofy.com/docs/

[Service]
Nice=19
IOSchedulingClass=best-effort
IOSchedulingPriority=7
Type=simple
ExecStart=/usr/bin/lynis audit system --cronjob

[Install]
WantedBy=multi-user.target

#################################################################################
```

__Step 3.__ 
Create timer unit (/etc/systemd/system/lynis.timer)

```
#################################################################################
#
# Lynis timer file for systemd
#
#################################################################################
# Do not remove, so Lynis can provide a hint when a newer unit is available
# Generator=lynis
# Version=1
#################################################################################

[Unit]
Description=Daily timer for the Lynis security audit and vulnerability scanner

[Timer]
OnCalendar=daily
RandomizedDelaySec=1800
Persistent=false

[Install]
WantedBy=timers.target

#################################################################################
```

__Step 4.__
Enable the timer
Tell systemd you made changes: systemctl daemon-reload
Enable and start the timer (so no reboot is needed): systemctl enable --now lynis.timer

__Step 5.__ 
Optional - Customize

Want to override the timer? Run: systemctl edit lynis.timer
Note: set the timer by first resetting it, then set the preferred value
```
[Timer]
OnCalendar=
OnCalendar=*-*-* 03:00:00
```
Just check your logs and/or root user alerts to review lynis audit results.

####################
#                  #
# #####  #   ###   #
#   #    #   ###   #
#   #    #   #     #
####################
TIP You need to literaly create the timer file, then restart the daemon, open file again, copy bottom half to top half like normal with drop in systemd files.
Restart the daemon again
Open file again, add the time schedule, save, restart the daemon. 
And now; It's set correctly.

# Journald
- - - -
### Locate and Interpret System Log Files and Journals
* `journald` * Responsible for event logging; records events from log files, kernel messages, etc.
	»» Data does not persist after reboot
	»» Can be configured for persistence in _etc_journald.conf
	»» Temporary log location: `/run/log/journal`
	»» Persistent log location: `/var/log/journal`
* `journalctl`
	»» `-n` * Set number of lines to show
	»» `-x` * Provide explanation text, if available
	»» `-f` * Show last ten events; continues listening
	»» `-b` * Show messages from current boot only
	»» `-p` * Show message priority type
	»» `_SYSTEM_UNIT=service` * Get events related to service
	»» `—since=yesterday` * Get events since defined time
	»» `—until=00:00:00` * Get event from before defined time
* Find information about system boot:
»» `systemd-analyze`
»» `systemd-analyze` blame
- - - -

#linux/commands
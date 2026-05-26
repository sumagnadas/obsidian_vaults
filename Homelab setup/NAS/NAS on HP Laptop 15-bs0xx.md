## Before installation of OMV
---
- Cleared baba's documents to the 1TB HGST HDD 
## During installation from ISO (installation and setup)
---
- English -> language
- Hostname set to `nas-hp-laptop-15`
- American English -> keymap
- no root pass
- users
	- sumagnadas
## After installation
---
- install tailscale
- enable ssh for my user by adding to `_ssh` group
- add public key authentication
  ```bash
  ssh-keygen -t ed25519 # generate a key pair
  ssh-copy-id -i id_nas_ed25519.pub sumagnadas@nas-hp-laptop-15 # copy the identity file to the main device
  ```
  - Disable password based authentication.
  - Set up docker engine
  - Setup immich (not at all recommended with 4gb ram, **NOT AT ALL**  because of the ML models)
  - Add static IP in the router settings for the NAS to 192.168.29.65
  - Setup software Wake-on-Lan (to mitigate constant wear on the USB controllers)
	  - Check using `ethtool` if NIC supports WoL.
	  - Since its not exactly a Wake-on-Lan as baba will use it so sleep during night and stay always on during day => cron task at 12:30 AM, computer wakes up on own.
		  - Use RTC wake alarm to wake up on 7:55AM by setting unix timestamp for when to wake up in `/sys/class/rtc/rtc0/wakealarm`
		    (RTC wake alarm: It is a hardware feature using the RTC or Real-Time Clock module which keeps track of the time to wake up the computer on a specified time)
		  - make the script suspend the computer after setting wake alarm.
	- To wake up from laptop from same network, `wakeonlan <mac-addr>`
	- (To set up) ESP32 to do the job of `wakeonlan` with tailscale.
- sdb (Seagate Expansion USB) - APM not supported via `hdparm`
  spin-down not configurable - mitigated by system sleep schedule (wanted to configure spin-down to not happen because of constant wear on the disk)
- Set up `audiobookshelf` for self hosted library.
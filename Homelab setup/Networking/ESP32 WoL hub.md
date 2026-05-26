## Setup and steps
- Set up ESP-IDF
- Clone and upload [Microlink](https://github.com/CamM2325/microlink)
- Microlink is BS with v6.1; might upgrade later from v5.3.5 :)
## Wake-on-Lan from scratch
- Open a UDP socket
- Setup for global broadcast on the network with `sockaddr_in` object as:
  ```C
  struct sockaddr_in{
	  uint16_t sin_family = AF_INET;
	  uint16_t sin_port = 9;
	  uint32_t sin_addr = 255.255.255.255; // GLOBAL BROADCAST ADDRESS FOR DHCP and sending messages to all
	  uint8_t __pad[8];
  }
  ```
- Set up data packet as follows :-
	- Size of packet: 17x6 bytes (17 rows x 6 columns)\[not a matrix but an array]
	- 0-th row = 0xFF  => `packet[0,1,2,3,4,5] = 0xFF`
	- 1-th -> 16-th row = MAC address of 6 bytes => 
	  ```c
	  for(int j=0;j<6;j++) packet[i*6+j] = mac_addr[j]; 
	  // mac_addr = uint8_t[6] array, i = 1 to 16
	  ```
- Send the packet using the UDP socket with the sockaddr_in object and data packet as constructed.
***VOILA!! It's done.*** 

***Actual Implementation***: [esp_home_hub](https://github.com/sumagnadas/esp_home_hub)

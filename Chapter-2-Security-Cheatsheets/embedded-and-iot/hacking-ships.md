# Shodan searches

- org:"Inmarsat Solutions US"
  - Login for Globe wireless
- title:"sailor 900"
  - Sat antenna details
  - Exploit: https://www.exploit-db.com/exploits/35932
  - Default credentials: admin/1234
- html:commbox
  - KVH CommBox terminals
  - Vessel name / network structure leaked
  - "Show Users" link (or can request the content by appending /rest.php?action=QCgetActiveUsers)

# Vuln Ship Tracker

- https://ptp-shiptracker.herokuapp.com/

# Terms
- `ECDIS` are the electronic chart systems that are needed to navigate.
- `AIS transceiver` - system that ships use to avoid colliding with each other.
- `NMEA 0183` messages
  - Ethernet and serial networks are often ‘bridged’ at several points (GPS,satcom terminal, ECDIS)
  - OT systems are used to control the steering gear, engines, ballast pumps and lots more.
  - They communicate using  NMEA 0183 messages.
  - No message authentication, encryption or validation (only 2 byte XOR checksum)

# Attack Patterns

- Spoof the `ECDIS` using the vulnerable config interface, 'grow' the ship and 'jump' it in to the shipping lanes.
  - Other ships AIS will alert the ships captain to a collision scenario
- MitM and change NMEA 0183 messages to read differently
  - Ex: change the rudder command by modifying a GPS autopilot command

> https://www.pentestpartners.com/security-blog/osint-from-ship-satcoms/
> https://www.pentestpartners.com/security-blog/hacking-tracking-stealing-and-sinking-ships/

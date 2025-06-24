# urh

Universal Radio Hacker: investigate wireless protocols like a boss

```
- hardware interfaces for common Software Defined Radios
- easy demodulation of signals
- assigning participants to keep overview of your data
- customizable decodings to crack even sophisticated encodings like CC1101 data whitening
- assign labels to reveal the logic of the protocol
- fuzzing component to find security leaks
- modulation support to inject the data back into the system
- simulation environment to perform stateful attacks
```

- GitHub: https://github.com/jopohl/urh
- PDF Guide: https://github.com/jopohl/urh/releases/download/v2.0.0/userguide.pdf
- Video Guide: https://www.youtube.com/watch?v=kuubkTDAxwA&index=1&list=PLlKjreY6G-1EKKBs9sucMdk8PwzcFuIPB
- Wiki: https://github.com/jopohl/urh/wiki

# rtl_433

Program to decode traffic from Devices that are broadcasting on 433.9 MHz like temperature sensors

- GitHub: https://github.com/merbanan/rtl_433
- Blog Posts: https://www.rtl-sdr.com/tag/rtl_433/
- RPi and RRDTool: https://raspberrypiandstuff.wordpress.com/2017/08/04/rtl_433-on-a-raspberry-pi-made-bulletproof/

# gqrx

Software defined radio receiver powered by GNU Radio and Qt.

- GitHub: https://github.com/csete/gqrx

# IMSI-catcher

This program show you IMSI numbers of cellphones around you.

- GitHub: https://github.com/Oros42/IMSI-catcher

# AirplaneJS

An SDR app written in JavaScript that picks up ADS-B radio signals from airplanes and plots them in real time on a map in your browser

- GitHub: https://github.com/watson/airplanejs

# srsLTE

srsLTE is a free and open-source LTE software suite developed by SRS

- GitHub: https://github.com/srsLTE/srsLTE

# USB HID Security

script handling packets coming in from GNURadio via ZMQ and doing some decoding
- https://twitter.com/assortedhackery/status/1076886102466801664


- GitHub: https://github.com/miek/milight/blob/master/packet_handler.py


- USB HID Keyboard: https://github.com/mame82/P4wnP1_aloa/blob/master/hid/keyboard_globals.go#L6
- Logitech: https://twitter.com/mame82/status/1077191504412721152
- Logitech Unifying After patches: https://twitter.com/mame82/status/1093859460102131717
  - https://threatpost.com/logitech-keystroke-injection-flaw/139928/
- GreHack 2018: Trap Your Keyboard 101 - Marion Lafon: https://www.youtube.com/watch?v=bXMx6lt3Gz0

# QSpectrumAnalyzer

Spectrum analyzer for multiple SDR platforms (PyQtGraph based GUI for soapy_power, hackrf_sweep, rtl_power, rx_power and other backends)

https://github.com/xmikos/qspectrumanalyzer
https://twitter.com/YashinMehaboobe/status/1092150871360712704

# Resources

- LoRa Reverse Engineering and AES EM Side-Channel Attacks using SDR: https://archive.fosdem.org/2018/schedule/event/sdr_lora_aes/attachments/slides/2357/export/events/attachments/sdr_lora_aes/slides/2357/LoRa_AES_Security_SDR_FOSDEM_2018.pdf
  - https://www.youtube.com/watch?v=Q-0u87eFAm8
- TALKS FROM THE 2018 GNU RADIO CONFERENCE: https://www.rtl-sdr.com/talks-from-the-2018-gnu-radio-conference/
- Picking a Needle in a Haystack: Detecting Drones via Network Traffic Analysis: https://arxiv.org/pdf/1901.03535.pdf
- Listening in to a DECT Digital Cordless Phone with a HackRF: https://www.rtl-sdr.com/listening-in-to-a-dect-digital-cordless-phone-with-a-hackrf/
- Building a Carbon Fibre Dual Band Yagi Antenna for Amateur Radio Satellites with 3D Printed Parts for 20€: https://www.rtl-sdr.com/building-a-carbon-fibre-dual-band-yagi-antenna-for-amateur-radio-satellites-with-3d-printed-parts-for-20e/
- Eavesdropping on DECT6.0 Cordless Phones with a HackRF and GR-DECT2: https://www.rtl-sdr.com/youtube-tutorial-eavesdropping-on-dect6-0-cordless-phones-with-a-hackrf-and-gr-dect2/
- Implementing your own mobile phone: https://twitter.com/G33KatWork/status/1078580869844140032
- Breaking LTE on Layer Two: https://alter-attack.net/
- PCILeech uses PCIe hardware devices to read and write from the target system memory: https://github.com/ufrisk/pcileech

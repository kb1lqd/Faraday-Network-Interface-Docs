# TUNTAP Network interfaces

TUNTAP interfaces provide a software method to simulate a network interface card. This provides a standard network connection familiar to Ethernet cards while providing raw output data at either the layer 2 or layer 3 level. Primarily used to encrypt or tunnel data prior to physically leaving a computer it is useful to FaradayRF because it creates a standard network interface where a stream of bytes/packets are both inputted and outputted. Standard tools, protocols, and programs can be used during development such as wireshark, TCP/UDP, and FTP without creating awkward custom interfaces.

## Hardware Interfaces
The diagram below shows a high level hardware interface for the TUNTAP implementation. TUNTAP is used as a standard application interface and for protocols such as TCP/UDP. If possible utilizing the adapter to perform Layer 2 functionality and truly removing most of the firmware functionality should be explored.

![A simple high level diagram of the hardware interfaces for a TUNTAP network interface.](images/Faraday_TUNTAP.png "Faraday TUNTAP System Hardware Interface Diagram")

## Network Layer Overview

This diagram details the layer stack and protocol/program/adapter that creates the respective layer.

![Layer stack diagram of the Faraday network data implementation using TUNTAP adapters.](images/Faraday-Layers-Overview.png "Faraday TUNTAP Network Layers")

The goal is to provide as much standard interfaces and protocols as possible and minimize custom programs and protocols. Faraday simply provides a bytes in / bytes out functionality and lets the higher level software and computer do the heavy protocol lifting. The TAP interface should take care of the MAC layer as well to allow multiple units in a single area.

A simple framing protocol accessible at the TAP layer may need to be implemented to allow unit configuration and health data. Alternatively, unit configuration may just implement a simple framing protocol at boot during the first seconds of initialization if this is easier.

#### Questions

* Do we need a TAP interface or can we skip it?
  * TAP interface is NOT needed and not useful per current understanding. It is best to implement a Layer 2 (datalink) either on Faraday hardware or in python.
* Can Faraday communicate between two units with only a byte buffer and relying on higher level programs to control flow of data?
  * A layer 2 is needed and Faraday would be responsible for fragmenting and reassembling packets from the Layer 3 TUN network adapter.
* How could we plan for Faraday to implement (Layer 2?) functionality in the future that allows TDMA(time division multiple access) or FHSS (frequency hop spread spectrum) using the CC1101 features?
  * Layer 2 needs to be implemented on Faraday or in python (weird, keep on Faraday for now). We need to create a mechanism/method for updating device settings.

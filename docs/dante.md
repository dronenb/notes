# Dante

Dante is the product name for a combination of software, hardware, and network protocols that delivers uncompressed, multi-channel, low-latency digital audio over a standard Ethernet network using Layer 3 IP packets.

## Behringer WING Internal Dante Card

- Behringer WING supports installing Brooklyn II module internally.
- This gives the ability to use Dante (with the built in network interfaces) AND an additional expansion card
- Tutorial: <https://www.youtube.com/watch?v=Z3lfnDy8F0I>

## Configuring Cisco SG300 for Dante

Guides to follow for configuring Dante networks. I have had good results combining the instructions from all three, as they have slightly different QoS settings, I combined both QoS tables and enabled QoS "Advanced Mode" as per Yamaha guide, as well as IP multicast group address + port settings, which is not included in the Shure guide.
  
- <https://service.shure.com/s/article/configuring-sg300-switch-for-shure-devices-and-dante?language=en_US>
- <https://usa.yamaha.com/products/contents/proaudio/docs/dante_network_design_guide/301_setting_sg300.html>
- <https://global.audinate.com/learning/faqs/mac-osx-shows-listening-under-the-clock-status-tab-in-dante-controller>

## External Clock Sync

There is an "Enable Sync to External" option in the "Clock Status" tab of Dante Controller that will allow your preferred leader to sync its clock with an external source (which could be the internal clock of the console).

## Redundancy for Click/Tracks Playback

- Recommendation for replacement for DVS for tracks: <https://www.sweetwater.com/store/detail/RUio16D--yamaha-ruio16-d-dante-usb-audio-interface>
  - This supports redundancy
  - Unlike some other similar products, it is class compliant (no drivers or software necessary, can upgrade macOS firmware without fear of breaking DVS)

## USB-C Network Adapter

- Audinate has some documentation on supported adapters [here](https://global.audinate.com/learning/faqs/dvs-usb-ethernet-adaptor-choice-for-macos-systems?lang=ko)
  - Their recommendation seems to be [this StarTech adapter](https://www.amazon.com/StarTech-com-USB-Ethernet-Adapter-Gigabit/dp/B07Z177Y2D)
- Apple Thunderbolt 2 to Ethernet combined with Apple Thunderbolt 2 <-> 3 adapter seems to work fine

## PCIe Card with Apple Silicon Support

- Focusrite RedNet PCIe-R card won't work on Apple Silicon: <https://focusritepro.zendesk.com/hc/en-gb/articles/8102250465426-Focusrite-RedNet-PCIe-PCIeR-Card-Compatibility-Statement>
- The replacement is the Focusrite RedNet PCIeNX, which DOES support Apple Silicon
  - <https://focusritepro.zendesk.com/hc/en-gb/articles/360021638059-Installing-the-Red-Thunderbolt-and-RedNet-PCIeNX-drivers-on-Apple-silicon-M1-M2-and-M3-systems>
  - <https://www.sweetwater.com/store/detail/RedNetPCIENX--focusrite-rednet-pcienx-128-by-128-pcie-audio-interface>

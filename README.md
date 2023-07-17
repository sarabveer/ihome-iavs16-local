# ihome-iavs16-local

The iHome iAVS16 has a nice clock + weather display. I want to use it without it connecting to Amazon Alexa or any other non-essential external service.

## Setup

The iAVS16 should be setup normally through the iHome iAVS app. I connected it to my IoT WLAN/VLAN.

## Required Services

The iAVS16 at minimum requires access to the `time.windows.com` NTP server and a webserver running on port 80.

The Nginx webserver serves two purposes:

- Acts as a HTTPing target for the iAVS16. 
  - The iAVS16 uses google.com, yahoo.com, and microsoft.com as HTTP ping targets, continuously pinging them 24/7. It is better if it pings a local webserver.
- Proxy requests to AccuWeather to show weather info on screen.

## Firewall

For the webserver, you will need to force all external HTTP traffic from the clock to the webserver. In pfSense, this can be done using NAT Redirection.

You will also need to allow UDP port 123 for NTP access to `time.windows.com`. In my case, I have a local NTP server, so all NTP requests are also redirected using NAT.

Since only udp/123 and tcp/80 ports are allowed, the clock won't be able to connect to Alexa or any other service.

## Music

To stream music, Bluetooth functionality still works. The [HomeAssistant LinkPlay Integration](https://github.com/nagyrobi/home-assistant-custom-components-linkplay) also works.

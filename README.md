# Where it all started...
## Problem
So at my home we have 3 WLANs. My father created these because the router is on the ground floor and the signal can't reach the top floor. So he set up a repeater on every floor and so we have 3 WLANs named:
1. ... Ground Floor
2. ... Middle Floor
3. ... Top Floor
## Target
### The target is to:
1. Create a single WLAN that is able to provide good connection throughout the whole house and perhaps the garden.
2. Remove possible obstacles for better connection.
3. Fix other network issues.
Also, I want to do everything with the **least amount of money and expense**.
## Access
### fritz.box
I have **root** access to **http://fritz.box** and **all sub-connections**.
### Tangible Access
I have full access to all network-related devices without any restrictions.
# The fixing journey
## Initial Situation
The problem is described in the first point of the README.md  
The **abbreviation** "UCR" stands for "**U**tility **C**onnection **R**oom". That is a room on the ground floor where we have most of our utility devices.
### Devices
| Device | Note | Location |
| ----- | ----- | ----- |
| [Fritz!Box 7490](https://fritz.com/apps/knowledge-base/FRITZ-Box-7490/) | The main router that serves the *Ground Floor* | UCR |
| [FRITZ!WLAN Repeater 450E](https://fritz.com/apps/knowledge-base/fritz-wlan-repeater-450e-int/) | The repeater that serves the *Middle Floor* | In a Middle Floor bedroom |
| WNR1000v3 | A router from "Netgear". I don't know what it serves or if we even use it. | Location is unknown |
| FRITZ!DECT Rep 100 #1 | A repeater from AVM, that also supports DECT. It probably serves the *Top Floor*, but I don't know for sure. | In the hallway on the Top Floor |

## Progress
- Created a Sketch for the MESH NETWORK
- Trying to find out which Device serves *Top Floor*
- Found MAC-ADRESS for WLANs:
  - Ground Floor: 34:31:C4:CF:B2:F5
  - Middle Floor: 34:81:C4:AD:98:06
  - Top Floor: C4:04:15:03:25:DA
- Found MAC-ADRESS for WNR1000v3:
  - C4:04:15:03:25:DB
  - It is the same that is from the Top Floor only the Last Letter changes from "A" to "B". I think the WNR1000v3 is the Router that serves the Top Floor.
 
### Table for all Device Details  
| Device | Ipv4 | MAC-ADRESS | Served Floor |
| ----- | ----- | ----- | ----- |
| Fritz!Box 7490 | 192.168.178.1 | 34:31:C4:CF:B2:F5 | Ground |
| FRITZ!WLAN Repeater 450E | 192.168.178.24 | 34:81:C4:AD:98:06 | Middle |
| WNR1000v3 | 192.168.178.21 | C4:04:15:03:25:DB | Top |

### Tabel for all WLAN Details  
| WLAN | MAC-ADRESS | Served from |
| ----- | ----- | ----- |
| Ground | 34:31:C4:CF:B2:F5 | Fritz!Box 7490 |
| Middle | 34:81:C4:AD:98:06 | FRITZ!WLAN Repeater 450E |
| Top | C4:04:15:03:25:DA | WNR1000v3 |  
    
- Next Try is to connect to WNR1000v3 via Ipv4 -> When in Ground- or Middlefloor, im stuck in a white Screen. When in Topfloor, it throws a `ERR_CONNECTION_REFUSED`.
  - I looked in `#netDev` it confirms that im connect over WNR1000v3. So i think the device blocks direct Web Access in Repeater-/Bridge-mode.
 
### Solution Attempt
My train of thought was that my dad probably tried building a MESH Network Using the Fritz!Box, The Repeater 450E, and the WNR1000v3 but couldn't get it work because the WNR1000v3 don't support AVM Mesh and so we ended up with three different WLANs. Because we already have a compatible Device (FRITZ!DECT Rep 100 #1) but it don't serve any WLANs Connection, My Plan is to just create a new MESH Network but replace the WNR1000v3 with the FRITZ!DECT Rep 100 #1.

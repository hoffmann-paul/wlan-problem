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

After some research i found out that Fritz DECT Repeater only support expansion for DECT-phone and Smart-Home. So unfortunately we have to buy a FRITZ!WLAN Repeater. Something like the 450E. Than i can Setup the correct MESH.

### Setup
1. Reset the 450E Repeater in the WebInterface (to clear any legacy standalone-AP configuration and old SSID "Middle Floor").
2. Place the 450E close to the Fritz!Box 7490 (in the UCR) for initial pairing.
3. Enable Mesh on the Fritz!Box 7490
   - Login to `fritz.box`
   - Go to **Home Network → Mesh → Mesh Settings**
   - Confirm "WLAN Mesh" is active (should be default for a Mesh Master)
4. Pair the 450E via WPS
   - Press the WPS/Connect button on the Fritz!Box
   - Within ~2 minutes, press the WPS button on the 450E
   - Wait for both LEDs to go solid (successful pairing)
   - Alternatively: use **Assistants → Set up WLAN Repeater** on the Fritz!Box web interface
5. Verify Mesh integration
   - Go to **Home Network → Mesh → Mesh Overview**
   - Confirm the 450E now shows the Mesh icon (green) next to its entry
   - Confirm it inherited the Fritz!Box SSID/password automatically
6. Move the 450E to its final position (Middle Floor bedroom, same as before)
   - Check the repeater's signal LED after moving — should indicate green/good signal
   - If red/weak, reposition closer to the stairwell/Fritz!Box direction
7. Purchase a Mesh-compatible FRITZ!WLAN Repeater to replace the WNR1000v3
   - Candidates: FRITZ!Repeater 600 (cheapest, 2.4 GHz only, no LAN port) or FRITZ!Repeater 1200 AX (dual-band, has Gigabit LAN port for future cabling)
8. Pair the new repeater via WPS (same procedure as step 4)
   - Initially set it up near the Fritz!Box, NOT directly in the Top Floor
   - Confirm solid Mesh connection in **Mesh Overview** before moving it
9. Move the new repeater to the Top Floor hallway (near/instead of the FRITZ!DECT Rep 100 #1 location)
    - Check signal LED after relocation, reposition if weak
10. Decommission the WNR1000v3
    - Disconnect from LAN 3
    - Power off / remove from the network entirely
11. Clean up leftover SSIDs
    - Remove any remaining standalone WLAN names ("Ground Floor", "Top Floor" if still broadcasting from old configs)
    - Confirm only **one** unified SSID exists network-wide
12. Optimize channel settings
    - Under **WLAN → Radio Channel**, set to "Automatic" on both bands to avoid self-interference between Fritz!Box and repeaters
13. Update firmware on all devices
    - Fritz!Box 7490, 450E, and new repeater under **System → Update**

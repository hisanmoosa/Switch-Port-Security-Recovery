# Switch port security and recovery | Project 13
A Cisco Packet Tracer lab protecting an office switch port with one sticky MAC address. It demonstrates authorised communication, a violation caused by a different PC, and manual recovery.

![Final lab topology](images/topology.png)

## Design
| Device | IPv4 address | Connection |
|---|---|---|
| STAFF-PC | 192.168.60.11/24 | Switch Fa0/1, protected |
| TEST-PC | 192.168.60.12/24 | Switch Fa0/2 |
| UNKNOWN-PC | 192.168.60.13/24 | Disconnected in final topology |

These PC addresses and port connections follow the lesson plan. All PCs use the same subnet; no gateway is required. The screenshot's switch display label is S1-OFFICE, while its supplied IOS hostname is Switch.

## Configuration and results
Fa0/1 is an access port with port security enabled. It permits one learned sticky MAC, 000A.F3E1.2128, belonging to STAFF-PC. A different source MAC on this port triggers shutdown.

| Stage | Evidence |
|---|---|
| STAFF-PC connected | Ping success reported; Secure-up, 1 sticky MAC, 0 violations |
| UNKNOWN-PC replaces STAFF-PC | Timeout reported; Secure-shutdown, 1 violation; source 0060.3EA1.25A2 |
| STAFF-PC reconnected, port reset | Ping success reported; Secure-up; configured sticky MAC unchanged |

Maximum one MAC and shutdown violation action are defaults here; the status output confirms both even though the running-config omits those default commands.

## Reproduce
Open [the Packet Tracer project](lab/Project-13-Switch-Port-Security.pkt). Review [configuration and recovery commands](configs/Build-and-Recover.txt). Before testing UNKNOWN-PC, verify the legitimate MAC is learned. Reconnect STAFF-PC before shutdown/no shutdown recovery. Save after sticky learning to retain the learned configuration.

[Final switch configuration](configs/Switch-Running-Config.txt) · [Recorded evidence](tests/) · [Combined evidence image](images/Project-13-Network-Evidence.jpg)

## Learning
Sticky learning initially trusts the device whose MAC it learns; it does not identify a person's account. MAC-based port security is not encryption or a replacement for stronger authentication. Here it demonstrates limiting source MAC addresses on one port, not securing every switch port.

## Evidence scope
The uploaded .pkt and topology screenshot are preserved unchanged. The .pkt was not independently run or internally inspected during packaging. Text configurations omit repeated empty separators but retain all supplied statements. CLI image panels are recreated from supplied output, not original screenshots. Ping success and timeout are learner reports; no exact packet counts were supplied.

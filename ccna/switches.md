# CCNA - Switches

- Layer 1 addresses are subject to change, anything physically plugged in
- Layer 2 are mac addresses these will **never** change

Get access to the mac address table on a Cisco switch

```bash
Switch> enable
Switch# show mac-address-table
```

- Q: Which of the following addresses will a switch use to populate a CAM table
- A: source MAC address

A switch will use source Media Access Control (MAC) addresses to populate the
Content Addressable Memory (CAM) table. The CAM table, which is also called the
'switching table' is used by a switch to discorver the relationship between the
Open Systems Interconnection (OSI) Layer 2 addres of a device and the physical
port used to reach the device. The switch populates the CAM table by recording
the source MAC address of an inbound Layer 2 frame and the corresponding switch
port that the frame arrived on.

- Q: Which of the following addresses will a switch use to make forwarding
  decisions?
- A: destination MAC address

A switch will use destination MAC addresses to make forwarding descision.
Swiches make forwarding decisions base on the destintion MAC address contained
in the frame's header. The swich first searches the CAM table for an entry that
maches the frame's destination MAC address. The CAM table, which is also called
the switching table, used by a switch to discover the relationship between Layer
2 address of a device and the physical port used to reach the device. If the
frame's desitnation MAC address is not found in the table, the switch forwards
the frame to all its ports. except the port from which it recieved the frame. If
the destination MAC address is found in the table, the switch forwards the frame
to the appropriate port. The source MAC address is also recorded if it did not
previously exist in the CAM table.

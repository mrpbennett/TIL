# RAID explained

The best RAID setup for 6 drives depends on your specific needs and priorities,
such as data redundancy, performance, and capacity. Here are some common RAID
configurations for 6 drives:

## RAID 0:

RAID 0 stripes data across all drives, providing increased performance as data
is distributed across multiple disks. No data redundancy, so if one drive fails,
all data is lost. Best used for situations where performance is the primary
concern, and data is not critical or can be easily recreated.

## RAID 1+0 (RAID 10):

RAID 10 combines RAID 1 (mirroring) and RAID 0 (striping). Data is mirrored
across half of the drives, and then both mirrored halves are striped. Offers
good performance and high data redundancy. Can tolerate up to 3 drive failures
in the right configuration. Requires at least 4 drives, where 2 sets of 2 drives
are mirrored, and then both sets are striped.

## RAID 5:

RAID 5 stripes data across all drives and includes distributed parity
information for fault tolerance. Provides a good balance of performance and data
redundancy. Can tolerate a single drive failure without losing data.

## RAID 6:

RAID 6 is similar to RAID 5 but uses dual parity for added fault tolerance.
Offers better data protection than RAID 5 as it can tolerate up to 2 drive
failures without losing data. Slower write performance due to the overhead of
calculating and writing two parity sets.

## RAID-Z2 (ZFS):

RAID-Z2 is similar to RAID 6 but is specific to ZFS, a file system with advanced
data protection features. Provides dual parity for better data redundancy.
Suitable for systems running ZFS and wanting robust data protection.

## Unraid (Parity Storage):

Unraid is a unique storage solution that uses a single parity drive to protect
data. Allows mix-and-match drive sizes, and each drive can be accessed
independently. Can tolerate a single drive failure, but unlike traditional RAID,
it doesn't stripe data across all drives.

The best RAID setup for you depends on your specific use case and priorities. If
data redundancy and performance are essential, RAID 10 or RAID 6/RAID-Z2 may be
more suitable. If performance is the primary concern, RAID 0 might be an option.
It's essential to weigh the trade-offs between performance, capacity, and data
protection when choosing the right RAID configuration for your 6-drive setup.
Additionally, always ensure you have regular backups in place regardless of the
RAID configuration you choose, as RAID is not a substitute for backups.

## Storage -
### Azure Storage redundancy -
- **Locally redundant storage (LRS)** - copies your data synchronously three times within a single physical location in the primary region. LRS is the least expensive replication option, but is not recommended for applications requiring high availability or durability.
- **Zone-redundant storage (ZRS)** - copies your data synchronously across three Azure availability zones in the primary region. For applications requiring high availability, Microsoft recommends using ZRS in the primary region, and also replicating to a secondary region.
- **Geo-redundant storage (GRS)** - copies your data synchronously three times within a single physical location in the primary region using LRS. It then copies your data asynchronously to a single physical location in the secondary region. Within the secondary region, your data is copied synchronously three times using LRS.
- **Geo-zone-redundant storage (GZRS)** - copies your data synchronously across three Azure availability zones in the primary region using ZRS. It then copies your data asynchronously to a single physical location in the secondary region. Within the secondary region, your data is copied synchronously three times using LRS.
- **Read access to data in the secondary region for Geo-redundant storage (GRS) and Geo-zone-redundant storage (GZRS)** - **RA-GRS & RA-GZRS** -
Geo-redundant storage (with GRS or GZRS) replicates your data to another physical location in the secondary region to protect against regional outages. However, that data is available to be read only if the customer or Microsoft initiates a failover from the primary to secondary region. When you enable read access to the secondary region, your data is available to be read at all times, including in a situation where the primary region becomes unavailable. For read access to the secondary region, enable read-access geo-redundant storage (RA-GRS) or read-access geo-zone-redundant storage (RA-GZRS).
- Ref - https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy
### Performance Tiers In Storage Account - 
- **Standard**
Standard storage is backed by magnetic hard drives like HDD and provides the lowest cost per GB. they are best for applications that required bulk of data storage where data is access infrequently because read-write speeds are less as compare to premium.

- **Premium**
Premium storage accounts are backed by SSD (Solid-state drives) and offers low latency performance. They are mostly used with high-end systems and high-intensity applications like databases. You can not switch from standard storage account to premium storage account, you must create a new storage account with premium or standard base on your requirement and then copy the data

### Storage Account types -
. <br/>
![image description](https://github.com/arghya-roy/installs_and_notes/blob/main/Azure/images/Screenshot%20(53).png)

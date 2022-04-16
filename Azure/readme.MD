## Storage -
### Azure Storage redundancy -
- **Locally redundant storage (LRS)** - copies your data synchronously three times within a single physical location in the primary region. LRS is the least expensive replication option, but is not recommended for applications requiring high availability or durability.
- **Zone-redundant storage (ZRS)** - copies your data synchronously across three Azure availability zones in the primary region. For applications requiring high availability, Microsoft recommends using ZRS in the primary region, and also replicating to a secondary region.

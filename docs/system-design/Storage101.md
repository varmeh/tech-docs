# Storage 101

- [Storage 101](#storage-101)
  - [Block Storage](#block-storage)
    - [Characteristics](#characteristics)
    - [Advantages](#advantages)
    - [Use Cases](#use-cases)
    - [Examples](#examples)
  - [File Storage](#file-storage)
    - [Characteristics](#characteristics-1)
    - [Advantages](#advantages-1)
    - [Use Cases](#use-cases-1)
    - [Examples](#examples-1)
  - [Object Storage](#object-storage)
    - [Characteristics](#characteristics-2)
    - [Advantages](#advantages-2)
    - [Use Cases](#use-cases-2)
    - [Examples](#examples-2)
  - [Comparison Summary](#comparison-summary)
    - [Choosing the Right Storage](#choosing-the-right-storage)

Storage can be broadly classified into 3 categories

- Block Storage
- File Storage
- Object Storage

## Block Storage

### Characteristics

- `Low-Level Storage`: Data is stored in fixed-sized blocks, each with a unique identifier.
- `Raw Storage`: Provides raw storage volumes that can be partitioned, formatted, and mounted as individual drives.
- `High Performance`: Often used for high-performance applications due to low latency and high I/O throughput.

### Advantages

- `High Performance`: Suitable for performance-sensitive applications like databases and virtual machines.
- `Flexibility`: Can be used to create file systems or databases by the operating system or applications.
- `Reliability`: Often comes with features like replication and snapshots for data protection.

### Use Cases

- `Databases`: High-performance databases like SQL or NoSQL databases.
- `Virtual Machines`: Storage for virtual machines in cloud environments.
- `Transactional Applications`: Applications requiring fast and consistent I/O operations.

### Examples

- `Amazon EBS (Elastic Block Store)`
- `Google Persistent Disk`
- `Microsoft Azure Disk Storage`

## File Storage

### Characteristics

- `Hierarchical Structure`: Data is stored in files organized into directories and subdirectories.
- `File System Interface`: Accessed using standard file system protocols like NFS (Network File System), SMB (Server Message Block), or CIFS (Common Internet File System).
- `Ease of Use`: Simple to use and manage, ideal for storing and sharing documents and other file-based data.

### Advantages

- `User-Friendly`: Easy to navigate and manage using familiar file system operations.
- `Compatibility`: Compatible with many applications and operating systems.
- `Shared Access`: Supports concurrent access and sharing among multiple users and applications.

### Use Cases

- `Document Storage`: Storing and sharing documents, images, videos, and other file types.
- `Home Directories`: User home directories in enterprise environments.
- `Media Workflows`: Collaborative workflows in media and entertainment.

### Examples

- `Amazon EFS (Elastic File System)`
- `Google Filestore`
- `Microsoft Azure Files`

## Object Storage

### Characteristics

- `Flat Structure`: Data is stored as objects in a flat namespace, each with a unique identifier (key).
- `Metadata-Rich`: Each object can have extensive metadata for better management and retrieval.
- `Scalability`: Highly scalable, capable of storing vast amounts of unstructured data.

### Advantages

- `Scalability`: Easily scales to store petabytes and beyond.
- `Durability`: Designed for high durability with multiple copies of data stored across different locations.
- `Cost-Effective`: Typically offers lower cost per gigabyte compared to block and file storage.

### Use Cases

- `Backup and Archiving`: Long-term storage for backups, archives, and compliance data.
- `Big Data`: Storage for big data analytics and machine learning datasets.
- `Content Delivery`: Storage for static content like images, videos, and static websites.

### Examples

- `Amazon S3 (Simple Storage Service)`
- `Google Cloud Storage`
- `Microsoft Azure Blob Storage`

## Comparison Summary

| Feature             | Block Storage                          | File Storage                             | Object Storage                           |
|---------------------|----------------------------------------|------------------------------------------|------------------------------------------|
| Structure           | Blocks                                 | Files and directories                    | Objects with metadata                    |
| Interface           | Raw device access                      | File system protocols (NFS, SMB)         | RESTful APIs                             |
| Performance         | High                                   | Moderate                                 | Variable (depends on usage)              |
| Scalability         | Moderate                               | Moderate                                 | Very high                                |
| Use Cases           | Databases, VMs, transactional apps     | Document storage, home directories, media workflows | Backup, archiving, big data, content delivery |
| Examples            | Amazon EBS, Google Persistent Disk, Azure Disk Storage | Amazon EFS, Google Filestore, Azure Files | Amazon S3, Google Cloud Storage, Azure Blob Storage |

### Choosing the Right Storage

- `Block Storage`: Choose block storage for high-performance requirements such as databases and VMs where low latency and high IOPS are critical.
- `File Storage`: Choose file storage for scenarios where you need to share files across multiple users and applications, or where the hierarchical structure is beneficial.
- `Object Storage`: Choose object storage for highly scalable and durable storage needs, especially for unstructured data, backups, and big data applications.

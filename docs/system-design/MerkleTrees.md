# Merkle Tree

- [Merkle Tree](#merkle-tree)
  - [**1. What is a Merkle Tree?**](#1-what-is-a-merkle-tree)
    - [**How It Works**](#how-it-works)
  - [**2. Key Features**](#2-key-features)
  - [**3. Scenarios \& Applications**](#3-scenarios--applications)
    - [**1. Version Control Systems (e.g., Git)**](#1-version-control-systems-eg-git)
    - [**2. File Synchronization (e.g., Dropbox, rsync)**](#2-file-synchronization-eg-dropbox-rsync)
    - [**3. Blockchain \& Distributed Systems**](#3-blockchain--distributed-systems)
    - [**4. Backup Systems**](#4-backup-systems)
    - [**5. Peer-to-Peer Networks (e.g., BitTorrent)**](#5-peer-to-peer-networks-eg-bittorrent)
    - [**6. Anti-Virus Scanners**](#6-anti-virus-scanners)
  - [**4. Example Workflow**](#4-example-workflow)
  - [**5. Limitations**](#5-limitations)
  - [**Alternatives**](#alternatives)

Also called a **hash tree**.

It is designed to efficiently verify the **integrity** and **consistency** of data structures, such as *file systems*, by using cryptographic hashes (like CRC, though Merkle Trees typically use stronger hashes like SHA-256).

## **1. What is a Merkle Tree?**  

A Merkle Tree is a hierarchical structure where:  

- **Leaf nodes** represent the hashes of individual data blocks (e.g., files, using their CRC).  
- **Non-leaf nodes** represent the hash of their child nodes’ combined hashes.  
- The **root node** (top-level hash) uniquely represents the entire dataset (e.g., a folder or directory).  

### **How It Works**  

1. **Hashing Files**:  
   - Each file in a folder is hashed (e.g., CRC, SHA-1).  
   - Example: `File A → Hash_A`, `File B → Hash_B`.  

2. **Building the Tree**:  
   - Combine pairs of hashes and hash them again to create parent nodes.  
   - Repeat until a single root hash is generated.  

   ```
   Root Hash = Hash(Hash_A + Hash_B)
   ├── Hash_A (File A)
   └── Hash_B (File B)
   ```  

   For larger datasets, the tree becomes deeper:  

   ```
   Root Hash = Hash(Hash(Hash_A + Hash_B) + Hash(Hash_C + Hash_D))
   ├── Hash(Hash_A + Hash_B)
   │   ├── Hash_A (File A)
   │   └── Hash_B (File B)
   └── Hash(Hash_C + Hash_D)
       ├── Hash_C (File C)
       └── Hash_D (File D)
   ```  

3. **Comparing Trees**:  
   - To compare two folders, compare their **root hashes**.  
   - If root hashes match, the entire folder structure is identical.  
   - If they differ, traverse the tree to identify which subtree (or file) changed.  

---

## **2. Key Features**  

- **Efficient Comparison**: Identifies differences in O(log n) time instead of checking every file (O(n)).  
- **Tamper Detection**: Any change to a file propagates up the tree, altering the root hash.  
- **Partial Verification**: You can verify specific subtrees without needing the entire dataset.  

---

## **3. Scenarios & Applications**  

### **1. Version Control Systems (e.g., Git)**  

- Git uses a Merkle Tree-like structure to track file changes. Each commit generates a root hash, enabling efficient diffs between versions.  

### **2. File Synchronization (e.g., Dropbox, rsync)**  

- Detect changes in large folders by comparing root hashes. Only sync modified subtrees.  

### **3. Blockchain & Distributed Systems**  

- Bitcoin and Ethereum use Merkle Trees to verify transactions in blocks. Light nodes validate blocks without storing the entire blockchain.  

### **4. Backup Systems**  

- Identify incremental changes between backups by comparing root hashes.  

### **5. Peer-to-Peer Networks (e.g., BitTorrent)**  

- Verify file integrity during downloads by checking hashes of chunks against the root hash.  

### **6. Anti-Virus Scanners**  

- Quickly verify system files against known-good Merkle Trees to detect tampering.  

---

## **4. Example Workflow**  

Suppose you have two folders, **Folder A** and **Folder B**:  

1. Build a Merkle Tree for each folder.  
2. Compare root hashes:  
   - **Match**: Folders are identical.  
   - **Mismatch**: Traverse the tree to find conflicting subtrees/files.  
     - Example: If `Hash(Hash_A + Hash_B)` in Folder A differs from Folder B, check `Hash_A` and `Hash_B` to pinpoint the changed file.  

---

## **5. Limitations**  

- **Collision Risk**: Weak hashes (e.g., CRC) can produce the same hash for different files. Use cryptographic hashes (SHA-256) for security.  
- **Overhead**: Building the tree adds computational cost upfront.  

---

## **Alternatives**  

- **Flat Hash List**: A simple list of file hashes (less efficient for large datasets).  
- **Content-Defined Chunking**: Used in tools like `borg` or `restic` for deduplication.  

Merkle Trees are foundational in systems where data integrity, efficient comparison, and tamper-proofing are critical. Let me know if you'd like to dive deeper into a specific use case!

---
layout: post
title:  "Choosing a Protocol"
date:   2026-11-25 11:29:00 -0600
categories: UML
---

## Pros and Cons of Different Protocols

When a computer needs a package stored in or managed by a database system, the best transfer protocol depends on package size, network conditions, security requirements, and whether the computer requests the package or receives it automatically. In most designs, the database application retrieves the package and transfers it; exposing the database itself directly to client computers is usually unnecessary.

### HTTPS (download API)

- **Pros:** Encrypted in transit; widely supported by browsers, operating systems, and firewalls; works well over the internet; supports authentication and access control through an application; can provide checksums and resumable downloads.
- **Cons:** Requires an application or web server to manage downloads; large transfers need careful handling of retries and resume support; the server must be protected against unauthorized access and abusive requests.
- **Best for:** Most client-to-server package downloads, especially across networks or the public internet.

### SFTP

- **Pros:** Encrypts both the connection and credentials; reliable for automated file transfers; supports directory listings and file operations; widely available.
- **Cons:** Requires SSH account/key management and a reachable SFTP service; less convenient than a web API for fine-grained application permissions; clients need to know the remote file layout.
- **Best for:** Scheduled or managed transfers between systems when a file-transfer workflow is appropriate.

### SCP

- **Pros:** Encrypted and straightforward for copying files over SSH; useful in scripts and administrative workflows.
- **Cons:** Provides fewer file-management and transfer workflow features than SFTP; less suitable for end-user downloads or applications needing fine-grained access, auditing, and resume behavior.
- **Best for:** Simple, trusted system-to-system copies.

### SMB or NFS (shared network storage)

- **Pros:** Makes packages appear as files on a shared directory; convenient when several computers on a controlled local network need access; avoids implementing a separate download interface.
- **Cons:** Usually not appropriate to expose over the public internet; requires network and permission management; availability and performance depend on the shared storage; accidental changes or deletions can affect every client.
- **Best for:** Computers on a managed internal network that need access to the same package repository.

### FTP and FTPS

- **Pros:** FTP is simple and supported by many older tools; FTPS adds TLS encryption while retaining the FTP model.
- **Cons:** Plain FTP does not encrypt credentials or data and should not be used for sensitive packages; FTPS can be complicated to configure through firewalls because it uses separate control and data connections; neither is usually as convenient to integrate with modern application authentication as HTTPS.
- **Best for:** Compatibility with existing systems; choose FTPS rather than plain FTP when this approach is required.

### AMQP or another message queue

- **Pros:** Supports asynchronous delivery, retries, and decoupling the package producer from clients; useful when computers should be notified that a package is ready.
- **Cons:** Queues are designed for messages, not usually for carrying large package files; add operational complexity; clients still need a separate mechanism (often HTTPS or SFTP) to download the package.
- **Best for:** Notifying clients about new packages or coordinating delivery, alongside a file-transfer protocol.

### Database connections (such as JDBC or ODBC)

- **Pros:** Useful when the computer needs structured records or package metadata; standard drivers support queries and authentication.
- **Cons:** Database protocols are not generally designed for distributing large files to many clients; direct database access exposes more of the data service and complicates client permissions, scaling, and updates.
- **Best for:** Retrieving metadata or small structured results, not serving package binaries.

## Summary

For most systems, keep package metadata in the database and serve the package through an authenticated HTTPS download endpoint. Include a checksum so the client can verify the downloaded file. Use SFTP for controlled scheduled transfers, a shared directory for trusted computers on a local network, and a message queue only when asynchronous notification or coordination is needed.

## What I Choose
I would choose HTTPS because it is secure and widely supported, making it a good fit for this system. TLS encrypts the package while it is being transferred, and the application can authenticate each computer and check that it is allowed to download the requested package. HTTPS also works through most firewalls and can be used by standard clients without installing a special file-transfer tool.

The download endpoint should verify package permissions and provide a checksum so the client can confirm that the file arrived intact. For very large packages, it should also support resuming an interrupted download. This approach keeps the database protected behind the application rather than giving every client direct database access.
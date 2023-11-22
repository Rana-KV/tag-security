# Self-assessment
The Self-assessment is the initial document for projects to begin thinking about the
security of the project, determining gaps in their security, and preparing any security
documentation for their users. This document is ideal for projects currently in the
CNCF **sandbox** as well as projects that are looking to receive a joint assessment and
currently in CNCF **incubation**.

For a detailed guide with step-by-step discussion and examples, check out the free 
Express Learning course provided by Linux Foundation Training & Certification: 
[Security Assessments for Open Source Projects](https://training.linuxfoundation.org/express-learning/security-self-assessments-for-open-source-projects-lfel1005/).

# Self-assessment outline

## Table of contents

* [Metadata](#metadata)
  * [Security links](#security-links)
* [Overview](#overview)
  * [Actors](#actors)
  * [Actions](#actions)
  * [Background](#background)
  * [Goals](#goals)
  * [Non-goals](#non-goals)
* [Self-assessment use](#self-assessment-use)
* [Security functions and features](#security-functions-and-features)
* [Project compliance](#project-compliance)
* [Secure development practices](#secure-development-practices)
* [Security issue resolution](#security-issue-resolution)
* [Appendix](#appendix)

## Metadata

A table at the top for quick reference information, later used for indexing.

|   |  |
| -- | -- |
| Software | [TiKV](https://github.com/tikv/tikv/tree/master)  |
| Security Provider | No  |
| Languages | Rust, Java, Go, and C |
| SBOM |Link to the dependencies file: [Cargo.toml](https://github.com/tikv/tikv/blob/master/Cargo.toml) |
| | |

### Security links

Provide the list of links to existing security documentation for the project. You may
use the table below as an example:
| Doc | url |
| -- | -- |
| Security file | https://github.com/tikv/tikv/blob/master/security/Security-Audit.pdf |
| Default and optional configs | TBD - https://example.org/config |

## Overview
**TiKV (Ti Key-Value)**
TiKV is an open-source, distributed, and transactional key-value database. TiKV is scalable, low latency, and easy to use key-value database, and it is optimized for petabyte-scale deployments.

### Background

Key-value databases are optimized for fast data retrieval. They offer high performance for read/write operations, making them ideal for applications that require rapid access to data.

TiKV is part of the Cloud Native Computing Foundation and created by PingCAP; it features ACID-compliant transactional APIs and ensures data consistency and high availability through the Raft consensus algorithm, TiKV is designed to operate independently of heavy distributed file systems. The aim of the project is to provide a solution similar to Google Spanner and HBase, but with a focus on simplicity and ease of use for massive datasets.


### Actors
These are the individual parts of your system that interact to provide the 
desired functionality.  Actors only need to be separate, if they are isolated
in some way.  For example, if a service has a database and a front-end API, but
if a vulnerability in either one would compromise the other, then the distinction
between the database and front-end is not relevant.

The means by which actors are isolated should also be described, as this is often
what prevents an attacker from moving laterally after a compromise.

### Actions
These are the steps that a project performs in order to provide some service
or functionality.  These steps are performed by different actors in the system.
Note, that an action need not be overly descriptive at the function call level.  
It is sufficient to focus on the security checks performed, use of sensitive 
data, and interactions between actors to perform an action.  

For example, the access server receives the client request, checks the format, 
validates that the request corresponds to a file the client is authorized to 
access, and then returns a token to the client.  The client then transmits that 
token to the file server, which, after confirming its validity, returns the file.

### Goals
* TiKV clients let you connect to a TiKV cluster and use raw (simple get/put) API or transaction (with transactional consistency guarantees) API to access and update your data. TiKV clients interact with PD and TiKV through gRPC.
*  Scalability and consistency due to data being shared and replicated across multiple nodes (up to petabytes)
* Ensure data confidentiality and integrity in transit and at rest, using TLS
* Guarantee ACID properties for transactions to prevent data corruption
* Externally consistent distributed transactions to operate over multiple Key-Value Pairs
* Develop and maintain sophisticated access control systems to manage permissions effectively for different users and services interacting with TiKV
* Ensure compatibility and integration with widely used cloud-native security tools and platforms for seamless deployment in various environments 
* TiKV adopts MVCC(Multiversion Concurrency Control) which offers an extra layer of security in data management by maintaining multiple versions of data records. This approach helps to prevent conflicts and inconsistencies in a distributed environment where concurrent transactions occur. 


### Non-goals
* Not supposed to be a relational database (No SQL)
*  Implement those features which compromise performance without clear security benefits
*   Not good for “low-latency” reads and writes
*   Pursuing perfect security; the aim should be to mitigate risk to acceptable levels as perfect security is unattainable
*   Addressing end-user security, such as password manager or client side security, which are outside the database’s control
* Attempting to solve external network security issues that are beyond the scope of TiKV

## Self-assessment use

This self-assessment is created by the TiKV team to perform an internal analysis of the
project's security. It is not intended to provide a security audit of TiKV, or
function as an independent assessment or attestation of TiKV's security health.

This document serves to provide TiKV users with an initial understanding of
TiKV's security, where to find existing security documentation, TiKV plans for
security, and general overview of TiKV security practices, both for development of
TiKV as well as security of TiKV.

This document provides the CNCF TAG-Security with an initial understanding of TiKV
to assist in a joint-assessment, necessary for projects under incubation. Taken
together, this document and the joint-assessment serve as a cornerstone for if and when
TiKV seeks graduation and is preparing for a security audit.


## Security functions and features

* Critical.  A listing critical security components of the project with a brief
description of their importance.  It is recommended these be used for threat modeling.
These are considered critical design elements that make the product itself secure and
are not configurable.  Projects are encouraged to track these as primary impact items
for changes to the project.
* Security Relevant.  A listing of security relevant components of the project with
  brief description.  These are considered important to enhance the overall security of
the project, such as deployment configurations, settings, etc.  These should also be
included in threat modeling.

## Project compliance

Although TiKV does not provide explicit documentation regarding its compliance with specific regulations, it adheres to general best practices in security as an open-source distributed database. The documentation of TiKV, initially developed as an open source project targeting the Chinese market, does not explicitly address adherence to regulations such as GDPR and HIPAA, which hold greater relevance in Western markets. The documentation of project compliance can potentially expedite the process of adoption.

## Secure development practices

* Development Pipeline.  A description of the testing and assessment processes that
  the software undergoes as it is developed and built. Be sure to include specific
information such as if contributors are required to sign commits, if any container
images immutable and signed, how many reviewers before merging, any automated checks for
vulnerabilities, etc.
* Communication Channels. Reference where you document how to reach your team or
  describe in corresponding section.
  * Internal. How do team members communicate with each other?
  * Inbound. How do users or prospective users communicate with the team?
  * Outbound. How do you communicate with your users? (e.g. flibble-announce@
    mailing list)
* Ecosystem. How does your software fit into the cloud native ecosystem?  (e.g.
  Flibber is integrated with both Flocker and Noodles which covers
virtualization for 80% of cloud users. So, our small number of "users" actually
represents very wide usage across the ecosystem since every virtual instance uses
Flibber encryption by default.)

## Security issue resolution

***Note:*** When looking for methods of security issue resolution, there was no easily accessible area to discuss each protocol. This could potentially lead to mishaps like major security vulnerabilities being publicly shown as issues

* Bug Fixes or documentation improvement can implemented via a normal Github Pull Request workload
* To contribute “substantial changes”, RFC (request for comments) document provides a consistent and controlled path for new features to enter the project
  - RFC must provide summary, motivation, detailed design, drawbacks, alternative, and unanswered questions
* To report a vital security vulnerability, an email should be sent to the TiKV-security group( tikv-security@lists.cncf.io), and encrypted using the public PGP key
Disclosure Policy:
The received security vulnerability report shall be handed over to the security team for follow-up coordination and repair work.
After the vulnerability is confirmed, we will create a draft Security Advisory on Github that lists the details of the vulnerability.
Invite related personnel to discuss the fix.
Fork the temporary private repository on Github, and collaborate to fix the vulnerability.
After the fix code is merged into all supported versions, the vulnerability will be publicly posted in the GitHub Advisory Database.

Potential Vulnerabilities in Key-Value Database Systems:
Are there proper access controls that restrict unauthorized access to TiKV nodes and Placement Drivers by potentially malicious clients?
Potential Vulnerabilities related to the Raft Consensus Algorithm:
Are there proper integrity checks in place as the Leader Node is copying the client’s read/writes to the follower nodes?
Potential Vulnerabilities related to Multiversion Concurrency Control (MVCC) used in TiKV:
Is there any way to tamper with the timestamps in TiKV transactions?
Transaction timestamps are stored in the metadata associated with each key-value pair
If there is a conflict, i.e. two transactions trying to write to modify the same key, then only one transaction can be completed successfully, while others have to be retried.
To prevent conflicts, TiKV uses locks to control key access.
In theory, these timestamps are not encrypted (!!!), so the security of TiKV is contingent upon the security measures implemented at higher and lower levels (higher – Application level; lower – RocksDB)
Potential Vulnerabilities related to RocksDB, TiKV’s storage engine:
RocksDB may have vulnerable third-party dependencies, so if RocksDB has them, so does TiKV potentially.
RocksDB does not support Data-at-Rest encryption (DARE), which means encrypting any data stored in persistent storage
In fact, RocksDB does not have any built-in encryption features


## Appendix

***Known Issues OverTime***

* **Outdated Library Versions**: Some libraries used in TiKV were outdated and not actively maintained, posing a security risk. This issue highlighted the need for better patch management and attention to third-party dependencies
* **Database Encryption**: While the TiKV database encryption was evaluated, it was found to be using a basic XOR method, not yet production-ready. However, according to PingCap, it provides information about the encryption support in TiDB components. In TikV, it supports encryption at rest, and it allows TiKV to transparently encrypt data files using AES or SM4 in CTR mode.  
* **Codebase TODOs**: a large number of TODO blocks in the TiKV source code indicated incomplete functionalities, requiring further evaluation and integration to the project’s issue tracker. 
* **Fuzz Testing**: The implemented fuzzing tests were not properly run or evaluated at the time of the assessment, signaling a need for improvement in this area
* **Dependency Scanner Disabled**: the integrated dependency scanner was disabled due to a non-updated dependency, leading to potential security risks from unmonitored dependences
* **Lack of Bug Bounty Program**: While not a direct issue, the report suggested that implementing a bug bounty program could be beneficial for TiKV
* **High Apply Wait Tail Latency**: There was an issue with high latency in the apply wait tail, affecting performance
* **Performance regression**: a specific commit caused a 3-5% performance regression under certain workloads
* **False Positive in Slow Score**: This issue prevented the adoption of TiKV in production environments due to false positives in slow score measurement
* **Inconsistent Store Size After Enabling Titan**: Store size did not stabilize after enabling the Titan component
* **SST Importer Cleaning Requirement**: the SST Importer needed to clean the SST if no key-value needed to be saved and rewritten
* **CPU Usage of Snapshot-Worker Thread**: An enhancement was requested to reduce the CPU usage of this thread to prevent it from blocking scheduling
* **Slow Store Check Leader**: This bug potentially affected the ability of a store to advance resolve_ts
* **Titan Configuration Tuning**: This was listed as an enhancement issue to optimize the configuration of the Titan engine.
* **Different Store Size Among Nodes After Enabling Titan**: There were issues with varying store sizes among TiKV nodes after Titan was enabled
* **TiKV Out of Memory(OOM)**: There were reported instances of TiKV running out of memory. 

 
***CII Best Practices***
  > A CII Best Practice is a process or method that, when executed effectively, leads to enhanced project performance. TiKV has many practices that seem to fulfill this concept.
  * **Change Management**: TiKV, like many open-source projects, involves changes in code, features, and security practices. Effective change management is very crucial in the particular context for ensuring stable and secure updates to the software
* **Risk Management**: The process of identifying, assessing, and managing risks is really fundamental in developing softwares, especially a database like TiKV. This aligns with the CII best practices of project risk management, where potential impacts are evaluated to mitigate risks
* **Quality Management**: TiKV’s commitments to maintaining high-quality program and efficient processes showcases the CII best practices of quality management. This encompasses activities to improve efficiency and compliance, important for projects like TiKV which is widely used in various industries. 
* **Lessons Learned**: Continuous improvement in software development is the key to achieve CII Best Practices. It is also important to keep track of its previous issues and bugs, and learn from its past experiences. TiKV has done a great job for that.
  
***Case Studies***
  
* **JD Cloud and Ai** – A scalable database was required by cloud computing provider JD Cloud & AI to store metadata for its Object Storage Service (OSS). The metadata was exceeding the capacity of their MySQL database at an alarming rate. A globally ordered key-value store with the capacity to store enormous quantities of data was necessary. They decided to utilize TiKV following a scaled evaluation of their alternatives. TiKV is highly proficient in managing extensive datasets and offers support for petabyte-scale deployments. TiKV satisfied the performance, scalability, and defect tolerance criteria set forth by JD Cloud & AI after thorough testing. The application from TiKV has a latency of 10ms and a QPS of over 40,000. It has substituted MySQL as the metadata repository for their OSS. As of now, the principal database utilized by JD Cloud & AI for storing OSS metadata is TiKV.
  
* **SHAREit Group - SHAREit**, a widely used global content and advertising platform, aimed to improve their recommendation systems. Their focus was on achieving lower latency and an efficient SQL interface. The company chose to use TiKV for storing data online and interacting with their recommendation system. In addition, they have a plan to incorporate TiDB, which is an open-source HTAP database. This integration will allow them to use online machine learning, which means they can continuously enhance their recommendations by providing online training. TiKV and TiDB have successfully met their requirements for handling a large number of simultaneous write operations and providing a SQL interface. This has resulted in a simplified architecture for these systems. By adopting open source, engineers have been able to significantly decrease the amount of time they spend on feature engineering. For reference, this task took up around 50% of their time, but now it has been reduced to 25% or even less. SHAREit has improved its architecture and AI workflow for recommendations by adopting TiKV and TiDB and has resulted in a more efficient and streamlined process.
  
Deploys TiKV with TiDB:

* **Hulu** a popular video-streaming service, adopts TiKV with TiDB into their platform. There isn't a lot of detailed information about how Hulu specifically deploys TiKV and TiDB. However, based on the general understanding of TiKV, it is known that  TiKV is highly capable at managing distributed storage, while TiDB can handle SQL processing and transactions. Hulu could utilize it to store and organize large quantities of metadata associated with their video library and user interactions. TiKV lacks SQL interface and transactional support, so TiDB serves as a provider for these features. Hulu benefits from the combination of TiKV and TiDB because they provide a data infrastructure that is fast, scalable, and reliable. This enables Hulu to efficiently deliver content and recommendations to their audience.

***Related Projects / Vendors***
  
**Company: PingCAP; Offering: TiDB Cloud; Industry: DBaas.**
PingCAP is the company behind the open-source TiKV project and supplies continuous development of the project. TiDB Cloud is a database-as-a-service (DBaas) provided by PingCAP. It is built using TiKV and TiDB technologies. It offers fully-managed clusters of the open source TiKV database in the cloud. TiDB Cloud manages various operational tasks such as provisioning, upgrades, scaling, monitoring, and ensuring high availability. The main difference is that some users may prefer the convenience of TiDB Cloud as a service to the responsibility of administering their own TiKV environment. Overall,TiDB Cloud is intended for utilization by users who desire to operate it locally or as a service. Both projects give users the opportunity to leverage PingCAP's innovative technology. 


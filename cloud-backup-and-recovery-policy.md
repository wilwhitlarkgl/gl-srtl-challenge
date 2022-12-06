# Cloud Backup and Recovery Policy
This policy is designed to protect data within Graylog Cloud to be sure it is not lost and can be recovered in the event of an equipment failure, intentional destruction of data, or disaster.  It encompasses the Retention Cycle for Customers, detailing [Log Retention](#log-retention), [Alerting](#alerting), [Encryption](#encryption), [Replication](#replication) Across Sites and [Procedures](#procedures). General requirements for each of these components will be outlined and specifics related to our platform implementation (a.k.a. ["Platform Component Specifics"](#platform-component-specifics)) will be provided below.
## Scope
This policy applies to all data which provides the service delivered (“Graylog Cloud”) that is owned/leased and operated by Graylog, Inc. 
## General Requirements
These requirements are to be used by default. They can be overridden by the requirements
specific to a given component (see [Platform Component Specifics](#platform-component-specifics) below).
### Log Retention
* We must fully guarantee backups and restoration of backups for the full period of time that a customer retains data with us.
	* For example, if a customer bought 90 days of log retention, we have to keep a backup of all those 90 days to be able to fully restore all data.
* To protect customer data against datacenter failure, backups will be distributed to at least one additional datacenter location.
### Alerting
### Encryption
### Replication
### Procedures
# Platform Component Specifics
AWS OpenSearch Service
Snapshots in Amazon OpenSearch Service are backups of a cluster's indexes and state. State
includes cluster settings, node information, index settings, and shard allocation.
OpenSearch snapshots are incremental, meaning they only store data that changed since the
last successful snapshot.
The AWS OpenSearch Service provides both automated and manual snapshots. We use both.
Automated snapshots
Every hour: OpenSearch domain -> preconfigured S3 bucket

Automated snapshots are only for cluster recovery. They can be used to restore the
OpenSearch domain in the event of acquiring the red status or data loss. For more information,
see Restoring snapshots.
The OpenSearch service stores automated snapshots in a preconfigured Amazon S3 bucket at
no additional charge.
OpenSearch retains up to 336 of them for 14 days.
The objects in the preconfigured S3 bucket cannot be accessed directly.
NOTE: If a cluster becomes red, all automated snapshots fail while the cluster status persists.

Manual snapshots
Every 15 min: OpenSearch domain -> primary S3 bucket -> secondary S3 bucket.
Manual snapshots are utilized for cluster recovery, but could also be used for moving data from
one cluster to another.
As the name suggests, manual snapshot creation has to be initiated. They are stored in an S3
bucket that we provide, and standard S3 charges apply.
Manual snapshots must be initiated every 15 minutes to provide a more recent recovery point
than the automated snapshots.
Manual snapshots are retained for 90 days.
To protect us against accidental backup deletion or longer AWS region outages the S3 bucket
must be mirrored to a secondary bucket using S3 object replication.
# Ownership
This file and the policies contained herein are owned and maintained by the Cloud Services team. You can reach out to @wilwhitlarkgl if you have questions.

## Copilot instructions for NetApp Workload Factory for Databases documentation

### Repository overview
Product: NetApp Workload Factory for Databases

NetApp Workload Factory for Databases is an end-to-end database deployment and maintenance service that discovers, assesses, provisions, and manages Microsoft SQL Server, PostgreSQL, and Oracle databases on Amazon FSx for NetApp ONTAP (FSx for ONTAP). It provides built-in best practices for optimization, automatic thin cloning, cost savings analysis, and continuous well-architected monitoring.

### Repository structure
- `learn-databases.adoc` – Product overview: what Workload Factory for Databases is, supported configurations, integrated AWS services, and FSx for ONTAP benefits
- `quick-start-databases.adoc` – Getting started guide covering login, credentials, resource discovery, savings exploration, and well-architected setup
- `create-database-server.adoc` – Steps to deploy a new Microsoft SQL Server host on FSx for ONTAP using Quick create or Advanced create modes
- `create-postgresql-server.adoc` – Steps to deploy a new PostgreSQL server (HA or single instance) on FSx for ONTAP
- `create-database.adoc` – Steps to create a Microsoft SQL user database within a managed SQL Server instance
- `register-instance.adoc` – Steps to register Microsoft SQL Server instances and Oracle databases for continuous monitoring and management
- `manage-instance.adoc` – Overview of resource management capabilities for registered SQL Server and Oracle resources
- `explore-savings.adoc` – Savings calculator for comparing costs between EBS, FSx for Windows File Server, on-premises storage, and FSx for ONTAP
- `optimize-overview.adoc` – Well-architected framework analysis covering storage sizing, storage layout, storage configuration, compute, application, and resiliency for SQL Server and Oracle
- `optimize-configurations.adoc` – Steps to implement well-architected recommendations and fixes for registered database resources
- `protect-sql-server.adoc` – Steps to protect SQL Server workloads using NetApp Backup and Recovery with the Plug-in for Microsoft SQL Server
- `analyze-error-logs.adoc` – AI-powered (Amazon Bedrock) error log analysis for SQL Server and Oracle using the smart error log analyzer
- `monitor-databases.adoc` – Database job monitoring
- `create-sandbox-clone.adoc` – Steps to create a sandbox (thin) clone from a source database for dev, test, QA, and analytics
- `refresh-clone.adoc`, `revert-clone.adoc`, `check-clone-integrity.adoc`, `split-clone.adoc`, `delete-clone.adoc`, `view-clone-connection.adoc`, `connect-clone-cicd.adoc` – Clone lifecycle management tasks
- `deregister-instance.adoc` – Steps to deregister resources from Workload Factory management
- `automate-codebox.adoc` – Codebox automation: generating IaC snippets, REST API, AWS CLI, CloudFormation, and Terraform templates
- `whats-new.adoc` – Release notes landing page
- `_whatsnew/` – Release note snippets included into `whats-new.adoc`, one file per release
- `_include/` – Shared content snippets included across pages
- `known-limitations.adoc` – Current product limitations
- `support-registration.adoc`, `get-help.adoc` – Support and help resources
- `media/` – Screenshots and images referenced in documentation

### Product-specific context

**Architecture and components:**
- Workload Factory is the umbrella platform; *Databases* is one workload within it alongside Storage, AI, VMware, and others
- All database storage runs on *Amazon FSx for NetApp ONTAP (FSx for ONTAP)* — this is the required storage backend
- Databases run on *Amazon EC2* instances; Workload Factory connects to them via *AWS Systems Manager (SSM)*
- A *link* (also called a *Workload Factory link*) provides connectivity between Workload Factory and FSx for ONTAP file systems for advanced management and well-architected analysis
- *Codebox* is Workload Factory's built-in IaC co-pilot that generates REST API, AWS CLI, CloudFormation, and Terraform code for any operation
- *NetApp Backup and Recovery* is the integrated data protection service; it uses the *Plug-in for Microsoft SQL Server* (a host-side component) to back up and replicate SQL Server workloads
- *Amazon Bedrock* is required for the AI-powered error log analyzer feature

**Key concepts:**
- *Inventory*: the centralized view in Workload Factory where all discovered database hosts (SQL Server, Oracle, PostgreSQL) appear; hosts appear after AWS credentials with view permissions are added
- *Registration*: the process of onboarding a SQL Server instance or Oracle database into Workload Factory for full management (monitoring, cloning, well-architected analysis); requires instance/database authentication, operations and remediation permissions, and link association with the FSx for ONTAP file system; PostgreSQL hosts appear in the Inventory but are not registered for management
- *Well-architected assessment*: continuous or one-time analysis of SQL Server and Oracle configurations across five configuration categories — storage, compute, application, resiliency, and cloning
- *Sandbox clone*: a thin clone created from the most recent FSx for ONTAP snapshot of a source database, used for dev, test, QA, analytics, or training without affecting the source; clones must share the same FSx for ONTAP file system as the source
- *Savings calculator*: compares total cost of ownership (storage, compute, SQL licensing, snapshots, clones) for SQL Server workloads on Amazon Elastic Block Store (EBS), FSx for Windows File Server, or on-premises versus FSx for ONTAP
- *Quick create* and *Advanced create*: two deployment modes available for creating SQL Server hosts and databases; Quick create applies default best-practice settings while Advanced create allows full customization
- *Thin provisioning*: FSx for ONTAP volumes are thin-provisioned by default; new database files consume only a few MBs of the total allocated size

**Naming conventions and terminology:**
- The product is always *NetApp Workload Factory for Databases* (full name) or *Workload Factory for Databases* (short form); never "WFF" or "NWFF"
- The storage service is *Amazon FSx for NetApp ONTAP* or *FSx for ONTAP*; never "FSx" alone when referring to this specific service
- *Microsoft SQL Server* (full name) or *SQL Server*; never "MSSQL" in user-facing content
- *FCI* = Failover Cluster Instance (SQL Server deployment model)
- *Always On availability group* = SQL Server HA deployment model
- *ASM* = Automatic Storage Management (Oracle disk group management)
- *Data Guard* = Oracle HA/DR replication feature
- *SnapMirror* = NetApp replication technology used by Backup and Recovery for SQL Server protection
- *Workload Factory console* = the web UI; users can also access via *NetApp Console* or *Workload Factory API*
- Permissions in Workload Factory are tiered: *View, planning, and analysis* (discovery and assessment), *Operations and remediation* (management and fixes), and *Database host creation* (deployment)
- *CRR* = cross-region replication (FSx for ONTAP feature for disaster recovery)
- *MPIO* = Microsoft Multipath I/O (required OS setting for SQL Server on EC2 with FSx for ONTAP)
- *MAXDOP* = Maximum Degree of Parallelism (SQL Server configuration parameter)
- *dNFS* = Direct NFS (Oracle kernel NFS client for improved NFS performance)

### Typical user workflows

**Explore cost savings:** Access Databases dashboard → Open Explore savings tab → Use savings calculator (customization or detected hosts mode) → Review itemized cost comparison → Optionally deploy SQL Server on FSx for ONTAP from calculator

**Deploy a new database host:** Log in to Workload Factory → Select Databases workload → Deploy host → Choose Microsoft SQL Server or PostgreSQL → Select Quick create or Advanced create → Configure landing zone, VPC, storage (FSx for ONTAP), and database settings → Deploy via console or generate CloudFormation/Terraform via Codebox

**Register and manage existing resources:** Discover hosts in Inventory (requires View, planning, and analysis permissions) → Select instance → Authenticate SQL Server/Oracle credentials → Complete prerequisite checks (install AWS and NetApp PowerShell modules) → Associate Workload Factory link with FSx for ONTAP file system → Register instance → View in Inventory → Create databases, clones, or run well-architected assessment

**Well-architected continuous assessment:** Register resources → Associate a Workload Factory link → Grant View, planning, and analysis permissions → Review well-architected dashboard → Apply recommended configuration fixes

**Create and manage sandbox clones:** Register source SQL Server instance → Navigate to Sandboxes tab → Create new sandbox → Select source host, instance, and database → Select target host and instance (same FSx for ONTAP file system) → Configure mount point → Create clone → Manage lifecycle (refresh, revert, check integrity, split, or delete)

**Protect SQL Server with Backup and Recovery:** Register SQL Server instance with Windows credentials → Select Protect from Inventory → Workload Factory installs Plug-in for Microsoft SQL Server → Redirect to NetApp Backup and Recovery → Create protection policy with snapshot and replication settings

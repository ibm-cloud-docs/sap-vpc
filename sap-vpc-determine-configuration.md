---

copyright:
  years: 2020
lastupdated: "2020-06-25"

keywords: SAP NetWeaver, {{site.data.keyword.cloud_notm}}, RDBMS, SAP Application Performance Standards, SAPS, SAP Certified, database

subcollection: sap-vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}


# Determining your configuration
{: #determine_configuration}

With an SAP NetWeaver system, you have two deployment options:
  * Central system, which is a single-host installation (two-tier)
  * Distributed system, which is a multi-host installation (three-tier)

The options influence your choice of {{site.data.keyword.vsi_is_full}}. You might want to distribute your workload to different virtual servers or keep the workload on one virtual server for simplicity. For more information about provisioning a server, see [Setting up your infrastructure](/docs/sap-vpc?topic=sap-vpc-set_up_infrastructure).
{:shortdesc}

## Storage configuration
{: #storage_config}

Table 1 is a sample storage configuration for an `mx2-32x256` site using [{{site.data.keyword.block_storage_is_full}}](/docs/vpc?topic=vpc-block-storage-about) volumes with

  * 500 GB storage at 10,000 IOPS for the database
  * 2,000 GB for backup at 4,000 IOPS (medium performance)

| File system | Volume | Storage type | IOPS/GB | GB | \# IOPS |
| --- | --- | --- | --- | --- | --- |
| `/` | `vdal` | Pre-configured boot volume | N/A | 100 GB | 3,000 |
| `/boot` | `vda2` | Pre-configured boot volume | N/A | 0.25 GB | 3,000 |
| `/db2` | `vdd` (can vary) | Data volume | N/A | 500 GB | 10,000 |
| `backup/log` and `backup` | `vde` (can vary) | Data volume | 5 IOPS/GB | 2000 GB | 4,000 |
{: caption="Table 1. Sample storage configuration" caption-side="top"}

Table 1 shows a basic layout of the file system to support a Db2 installation. Generally, a Db2 installation has dedicated subdirectories which can be segmented into independent volumes. For example, `"/db2/<DBSID>"`, `"/db2/<DBSID>/log_dir"`, and several `"sapdata<n>"`, where the folder `"log_dir"` contains the online logs files of the database and the `"sapdata<n>"` contains the data itself. See for example, the Db2 documentation here: [Required File Systems for IBM Db2 for Linux, UNIX, and Windows](https://help.sap.com/viewer/ce9e270ad34949969c16d09d1b099a26/CURRENT_VERSION/en-US/713eb64f45c6448c8dbe8a51b85680ee.html){: external}.

You have to consider the total IOPS required for your installation and the performance characteristics of your database. An option is to collocate multiple directories into a single large volume with high IOPS, versus isolating directories into individual small volumes with an insufficient number of IOPS for the workload characteristics.

For an overview of all available storage profiles, see [VPC Block Storage Profiles](/docs/vpc?topic=vpc-block-storage-profiles). For databases running SAP NetWeaver, a minimum of 5 IOPS/GB is recommended.

## High-availability configuration and support
{: #ha}

The {{site.data.keyword.cloud_notm}} environment doesn't provide preconfigured high-availability (HA) scenarios. However, you can configure HA scenarios for SAP NetWeaver that are based on the HA extension for the operating system you choose.

The underlying {{site.data.keyword.cloud_notm}} environment is designed in a highly available format. This format helps prevent hardware or network level Single Points of Failure (SPOF). Physical network adapters in the underlying hardware are configured in a redundant layout. As a result, no requirements exist for redundant virtual interface network cards (vNICS) in the {{site.data.keyword.vsi_is_short}}. Attached storage volumes are connected through redundant adapters and are kept on highly available controllers with multiple copies on physically isolated hardware to improve resiliency and uptime.

The {{site.data.keyword.cloud}} Infrastructure as a Service (IaaS) for SAP offers you a robust compute environment, and requires the operating system (OS) deployment to contain the supported HA solutions from each OS vendor. The solution is based on the supported OS version, which is discussed in [SAP Note 2927211](https://launchpad.support.sap.com/#/notes/2927211){: external}.

The HA solution is restricted to custom OS images being used with third-party OS licenses (BYOL) at this time. Make sure to discuss HA details with [{{site.data.keyword.cloud_notm}} Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support) before you deploy. Also contact {{site.data.keyword.cloud_notm}} Support to help you set up any extra requirements if you require extra software licenses, access to different software repositories, or both. This information not only applies to HA software for SAP NetWeaver, but also to HA software for your relational database management system (RDBMS) - including the high-availability and disaster-recovery mechanisms for your RDBMS, for example, replication.

Set up procedures for both SAP software and your RDBMS don't differ from set up procedures in an on-premises environment and require the same (virtualized) hardware and software configuration steps. However, you need to consider that at this time, storage volumes attached to multiple VSIs simultaneously are not supported.

Red Hat Enterprise Linux (RHEL) or SUSE Enterprise Linux Server (SLES) are supported for SAP NetWeaver with HA on {{site.data.keyword.cloud_notm}} using custom OS images. These operating systems support both SAP NetWeaver and SAP HANA HA solutions. Minor restrictions exist in the {{site.data.keyword.cloud_notm}} environment, which are discussed in [High Availability Partner Information](https://wiki.scn.sap.com/wiki/display/SI/High+Availability+Partner+Information){: external} where you can also find more information about high-availability products that are certified by SAP partners for high-availability scenarios.

For non-HANA SAP databases (such as Db2), more information on HA fail-over and DR configurations are available in your database documentation.

As with on-premises installations, check the performance and latency requirements of the database product when you plan your deployment.

## Extra guidance for SAP NetWeaver high-availability in IBM Cloud VPC
{: #extra-guide}

[Fencing](#fencing) and [Network considerations](#network) provide guidance that you need to consider during your deployment. Apart from the considerations that are outlined, installing SAP NetWeaver and its database system in an HA environment doesn’t differ from other on-premises installations.

### Fencing considerations
{: #fencing}

Due to the nature of a cloud environment, network-based access to remote management devices through the Intelligent Platform Management Interface (IPMI) is not available, which is typically used for clustering fencing to verify a quorum.

In the absence of an IPMI-enabled device and shared storage devices, you need to implement other fencing mechanisms. For example, Diskless Storage-based Death (SBD) for Linux. For more information about Linux High Availability Cluster Concepts,see [Cluster concepts](http://www.linux-ha.org/wiki/Cluster_Concepts){: external}.

### Network considerations
{: #network}

The network setup that is attached to a virtual server through the underlaying hypervisor is highly available by its own design. However, further subnets need to be added to the virtual server nodes in a cluster to segregate cluster internal communication from other - client or application - communication. This setup can also be considered for DR scenarios that are implemented by the replication of a database.

Depending on the network requirements, such as replication scenarios in a DR setup, you must check the location of the connected devices and any new network requirements that are specific to the product in question. Check with {{site.data.keyword.cloud_notm}} Support to determine which solution works best for your business process needs.

## Next steps
{: #determine-configuration-next-steps}

  * [Setting up your IBM Cloud account structure](/docs/sap-vpc?topic=sap-vpc-account-structure)

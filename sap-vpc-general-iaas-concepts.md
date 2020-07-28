---

copyright:
  years: 2020
lastupdated: "2020-06-25"

keywords: SAP NetWeaver, {{site.data.keyword.cloud_notm}}, data centers, {{site.data.keyword.baremetal_short}}, deployment, VLANs, SAP Certified, database

subcollection: sap-vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# SAP NetWeaver certified virtual servers on IBM Cloud VPC infrastructure
{: #sap-netweaver-on-ibm-cloud-iaas-overview}

An Infrastructure-as-a-Service (IaaS) environment consists of many components - primarily compute, storage, and network from a specified region (such as the US) and a designated site location (also referred to as zone, which is a data center site).
{: shortdesc}


## Introduction
{: #vpc}

With a wide availability of regions and zones across North and South America, Europe, Asia, and Australia, you can provision cloud resources where (and when) you need them. Each {{site.data.keyword.vpc_full}} is created for a region with virtual servers that are hosted in a zone within the selected region. Each {{site.data.keyword.vpc_short}} is connected to the {{site.data.keyword.cloud_notm}} global private network, making data transfers faster and more efficient anywhere in the world.

For more information about {{site.data.keyword.cloud_notm}} data centers and points of presence (PoPs), see the [data center regions and zones map](https://www.ibm.com/cloud/data-centers/#datacentermap){: external}.

### IBM Cloud Generation 2 environment
{: #vpc-gen2}

The {{site.data.keyword.vpc_short}} is available for Generation 1 {{site.data.keyword.virtualmachinesshort}} that are based on Classic Infrastructure (formerly known as VPC on Classic) and with Generation 2 {{site.data.keyword.virtualmachinesshort}}. The Generation 2 compute platform is based on the latest infrastructure, and offers improved networking performance (up to 80 Gbps), five times faster provisioning than before, and a flexible selection of extra IaaS resources.

{{site.data.keyword.cloud_notm}} for SAP provides SAP-certified infrastructure by using the latest Generation 2 {{site.data.keyword.virtualmachinesshort}}.

### IBM Cloud VPC network connectivity
{: #vpc-network-connectivity}

{{site.data.keyword.vpc_short}} provides a robust network that is secured for an individual customer, with flexible compute.

A VPC is created within a specific region, and that region contains zones where subnets are created and resources are deployed. Each zone uses an address prefix, while subnets exist within this construct. You can define each subnet manually by choosing the IP range and the subnet mask, or you can choose the number of IP addresses needed.

For more information on VPC access for zone-to-zone, VPC-to-VPC, VPC-to-Classic Infrastructure, and VPC-to-on-premises data centers by using a VPC VPN Gateway, see [About Networking for VPC](/docs/vpc?topic=vpc-about-networking-for-vpc) and [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure).


### VPC storage
{: #vpc-storage}

{{site.data.keyword.block_storage_is_full}} uses input/output operations per second (IOPS) to determine storage needs. All storage is selected based on capacity (GB) and performance (IOPS) measurements and is required to meet a specific SAP NetWeaver configuration.

IOPS are measured based on 16 KB block size with a 50/50 read/write mix. In order to achieve a maximum I/O throughput, it's advisable to look at the tier and custom profiles available for storage, in order to find the optimal combination of size and IOPS.

Contact [{{site.data.keyword.cloud_notm}} Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support) for extension options if the storage options are insufficient for your workload.


## IBM Virtual Servers
{: #virtual-servers}

{{site.data.keyword.virtualmachineslong}} (Gen2) are virtual machines that run on an {{site.data.keyword.IBM_notm}}-managed hypervisor. {{site.data.keyword.virtualmachinesshort}} provide flexible compute by using a multi-tenant infrastructure for SAP workloads. The operating system and applications that run inside the {{site.data.keyword.virtualmachinesshort}} are managed by the account holder, either you, your customer, or your services partner depending on your busineess operations.

The {{site.data.keyword.virtualmachinesshort}} that are certified for SAP are available with instantaneous provisioning, and are offered in different profiles that define CPU and RAM combinations.

For more explanation information about Virtual Servers, see [What is {{site.data.keyword.vpc_short}}?](https://www.ibm.com/cloud/learn/vpc){: external}.

### Virtual Server operating systems
{: #vs-operating-systems}

For a list of operating systems and databases available for SAP NetWeaver-based system deployments, see [SAP Note 2927211](https://launchpad.support.sap.com/#/notes/2927211){: external}.

  An SAP S-user ID is required to access the SAP Note.
  {: note}

## Deployment and management
{: #deployment-and-management}

{{site.data.keyword.virtualmachinesshort}} (Gen2) are deployed from the VPC Infrastructure section of the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/login){: external}, or by generating an API key using {{site.data.keyword.cloud_notm}} CLI, by using {{site.data.keyword.cloud_notm}} APIs, or by using [{{site.data.keyword.cloud_notm}} Terraform](https://ibm-cloud.github.io/tf-ibm-docs/index.html){: external}.

Once provisioned and the server is running, your {{site.data.keyword.virtualmachinesshort}} are managed through the {{site.data.keyword.cloud_notm}} console, {{site.data.keyword.cloud_notm}} CLI, {{site.data.keyword.cloud_notm}} APIs, or {{site.data.keyword.cloud_notm}} Terraform.

## Support
{: #concept-support}

{{site.data.keyword.cloud_notm}} Customer Support handles any support questions and issues that might arise by using various outlets, including chat, phone, and case-based support. Customer support is offered at no cost to all {{site.data.keyword.cloud_notm}} customers and covers most cases that are placed each day. See [Getting Customer Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support) for more information.

---

copyright:
  years: 2020
lastupdated: "2020-06-25"

keywords: SAP NetWeaver, {{site.data.keyword.cloud_notm}}, network connectivity, VLANs, hybrid, STMS, SAProuter, SAP Solution Manager, SAP certified, database, {{site.data.keyword.vpc_full}}, {{site.data.keyword.vpc_short}}

subcollection: sap-vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Items to consider when planning your SAP landscape
{: #considerations}

The SAP systems that run on {{site.data.keyword.cloud}} Infrastructure-as-a-Service (IaaS) in an SAP landscape have specific requirements for connectivity, both between IaaS components, such as virtual servers, and to external systems. You also need to consider and size storage requirements that are specific to your SAP applications.
{:shortdesc}


## Provisioning a virtual server instance
{: #provisioning-instances}

When you plan to provision {{site.data.keyword.vsi_is_full}}, use the Configuration checklist to get the most out of your deployment.

|        Configuration checklist |
|-------------------|
|* Make sure that your account has the required user permissions. If you have authorization as an editor or admin on a VPC resource, then you also inherit authorization to create, delete, and modify {{site.data.keyword.virtualmachinesshort}} within that VPC.|
|* Check your [account limits](/docs/vpc?topic=vpc-quotas) for concurrent instances. |
|* Verify that your [SSH key](/docs/vpc?topic=vpc-ssh-keys#ssh-keys) is available.
|* Decide the region and zone of your {{site.data.keyword.vsi_is_short}}.|
|* Determine the subnets to connect the instance to.|
|* Consider the [SAP-certified Virtual Server profile options](/docs/sap-vpc?topic=sap-vpc-size_the_server#choose_server) combinations of vCPU and RAM for your workload. Profiles contain preconfigured instances that are ready to use in minutes. Be sure that your instances have the necessary resources to keep your workloads and your environment up and running.|
|* Select an operating system image for your instance. You can choose among the current catalog images or specify your own custom image. Please check in [SAP Note 2927211](https://launchpad.support.sap.com/#/notes/2927211){: external} if the selected operating system and its version is supported. |
|* Give your instances unique names. Decide upon a method to name the virtual server instances, making it easier to filter and search on them later. |
|* Decide how many auxiliary storage volumes you need. |
|* Determine how many network interfaces you need and which security group to attach to each interface.|
{: caption="Table 1. Configuration checklist for planning to provision instances" caption-side="top"}


## VPC network connectivity
{: #network_connectivity}

[{{site.data.keyword.vpc_full}} Gen2 network overview](/docs/vpc?topic=vpc-about-networking-for-vpc) demonstrates the connectivity for the environment. Issues with network connectivity can cause delays for your project if you do not plan properly, regardless of how you plan to use your system.

In general, {{site.data.keyword.cloud_notm}} Gen2 has a highly available, high-bandwidth network that is connected to every physical server, which serves the hypervisor. Each physical server (host) has a hypervisor that divides the network into virtual network interfaces (vNICs) that are attached to the virtual server.

Depending on the profile of your virtual server, the total available network bandwidth to the virtual server is in the range of 4 Gbps to 64 Gbps. It's important to consider that each vNIC has a maximum throughput of 16 Gbps, so to achieve maximum throughput, up to 4 vNICs must be attached to the virtual server.

If you need to connect to your virtual server through the public internet, in other words, inbound to a virtual server, you can order _Floating-IPs_ and attach them to the virtual server per vNIC.

If you want to connect to the public internet from your virtual serveer, in other words, outbound from a virtual server, you need to attach a _Public Gateway_ to the VPC. This gateway provides access to the internet for an entire subnet.

When a connection to the public internet is not acceptable because of security measures, you can deploy an IPsec Gateway into your VPC to connect to your virtual server. For more information, see [Accessing your infrastructure - VPC VPN Gateway](/docs/sap-vpc?topic=sap-vpc-access-infrastructure#vpc-vpn-gateway). Or, you can have an even closer integration into your backbone infrastructure by an {{site.data.keyword.BluDirectLink}}, for more information, see [Accessing your infrastructure - IBM Cloud Direct Link](/docs/sap-vpc?topic=sap-vpc-access-infrastructure#ibm-cloud-direct-link).

Server resources that are in {{site.data.keyword.cloud_notm}} Classic Infrastructure can be connected through _Transit Gateways_. These virtual devices are used to connect your private VLAN subnets in the _Classic Infrastructure_ to your VPC subnets.

Extra requirements exist in Classic Infrastructure networking to enable the _Transit Gateway_, be sure to review documentation before you change your Classic Infrastructure or VPC Infrastructure networking topology and configuration.
{:important}

Advise your networking department to contact [{{site.data.keyword.cloud_notm}} Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support) after determining the layout of your landscape and the connectivity that is required on the SAP application layer.
{: note}

## VPC network constructs
{: #vpc-network-constructs}

Each VPC zone uses an address prefix (more details available in [{{site.data.keyword.vpc_short}} network connectivity](/docs/sap-vpc?topic=sap-vpc-sap-netweaver-on-ibm-cloud-iaas-overview#vpc-network-connectivity)), with subnets within this construct. Each VPC subnet is for a specific zone.

{{site.data.keyword.cloud}} offers further protection mechanisms that can provide your {{site.data.keyword.vsi_is_short}} with a layer of security that you can configure and adapt anytime. Two key principles are:
  * **Network Access Control Lists (ACL)**: Available for use by all subnets in all zones, attached to a subnet providing subnet-level protection by limiting a subnet's inbound and outbound traffic.
  * **Security Groups**: Available for use by all subnets on all zones, attached to a vNIC of a virtual server providing instance-level protection by acting as a firewall to restrict a vNIC inbound and outbound traffic.

### Network Access Control List
{: #network-acl}

Network Access Control Lists (ACLs) are used to manage `allow` and `deny` rules on a subnet level. ACLs are used to manage network traffic between subnets, too. The default ACL for a subnet opens the subnet for all traffic. If you wanted more strict security measures, you would need to apply rules to the ACL. When adding rules, keep in mind that required services like DNS or OS patch/package download might be affected by those rules. For more information, see [Security in your VPC](/docs/vpc?topic=vpc-security-in-your-vpc).

### Security Groups
{: #security-groups}

A Security Group is a set of `allow` only firewall rules. You can apply these rules to one or more virtual servers. You can also create a default Security Group with Secure Shell (SSH) and ICMP (ping) during VPC creation, which will allow ICMP and SSH from any IP address. These rules should be restricted to the IP ranges from which you are planning to access the VPC. Rules should also allow for SAP application ports if you are planning to access them through the public internet.

For more information, see [Security in your VPC](/docs/vpc?topic=vpc-security-in-your-vpc).


### Subnets to separate traffic
{: #multiple-subnets}

If you want to separate different network traffic types in your landscape, either because of security restrictions or because of throughput considerations, you can configure and attach multiple subnets to your VPC. For the following use cases, make sure that all servers are on subnets with interconnectivity during deployment:
  *	Multiple servers that exchange data, such as a three-tier SAP database and SAP application instances on different virtual servers.
  *	Multiple systems that exchange large amounts of data.

### Hybrid setups
{: #hybrid}

{{site.data.keyword.cloud_notm}} for SAP can be thought of as an external data center when compared to traditional SAP environment setups using a data center provider. This is especially true if you're thinking of implementing a hybrid landscape with some SAP systems at an {{site.data.keyword.cloud_notm}} data center and other systems on site.  {{site.data.keyword.cloud_notm}} also provides a rich set of functions beyond hosting SAP systems. The following are specific configuration items that you need to consider as part of your project’s planning phase within a hybrid setup.

  *	[SAP Transport Management System (STMS)](https://www.sap.com/products/transportation-logistics.html){: external}. Configure STMS based on Transport Groups to prevent file sharing across data centers.
  *	[SAProuter](https://support.sap.com/en/tools/connectivity-tools/saprouter.html){: external}. Provides access to SAP Online Service System (OSS). Use your on-premises SAProuter to access the OSS. This SAProuter can be used through further SAProuter hops if IP-based routing is not allowed between your {{site.data.keyword.cloud_notm}}-based systems and your on-premises SAProuter. Alternatively, you might consider setting up another SAProuter that is based on one {{site.data.keyword.cloud_notm}}-based server with a public IP and connect it to the SAP OSS system through the internet.
  *	[SAP Solution Manager](https://support.sap.com/en/alm/solution-manager.html){: external}. Access to the SAP Solution Manager has different connectivity requirements between an SAP Solution Manager and its managed systems. The differences depend on your usage scenario. These scenarios require an understanding of the required network connectivity.
  * If you are deploying public gateways or floating IPs, you need to look into the details of Network Address Translation (NAT) and the behavior of SAP applications. Refer to the [SAP document on NAT](https://wiki.scn.sap.com/wiki/display/ABAPConn/NAT+and+RFC){: external} to consider potential issues on the application layer, especially in the SAP Remote Function Calls (RFCs).

## VPC Storage
{: #storage}

If you need more than the initially provisioned storage in your virtual server, you can attach extra volumes to a virtual server later. This storage can be detached and attached to other virtual servers at any time, but, only to one virtual server at the same time. No shared storage for virtual servers is available in VPC Gen2 at this time.

Storage volumes differ in performance, depending on their IOPS tier. You can select among 3, 5, and 10 IOPS/GB (see [Tiered IOPS profiles](/docs/vpc?topic=vpc-block-storage-profiles#tiers)). You can also select a [custom size](/docs/vpc?topic=vpc-block-storage-profiles#custom) (in GB and IOPS) that is based on the size of the storage. Since IOPS per GB is limited, you might not want to have your relational database management (RDBM) data storage on a separate volume from the relational database management system (RDBMS) log storage. This setup depends on the behavior of your application. By rule of thumb, for typical RDBMS-based applications, a 5 IOPS/GB profile is reasonable.

If your application uses dedicated key performance indicators (KPIs) on storage performance, it's advised that you test storage throughput before you begin software deployment. By using volume manager-based software RAID (like LVM), you meet almost every KPI.

## Next Steps

  * [Sizing the virtual server](/docs/sap-vpc?topic=sap-vpc-size_the_server)
  * [Determining your configuration](/docs/sap-vpc?topic=sap-vpc-determine_configuration)

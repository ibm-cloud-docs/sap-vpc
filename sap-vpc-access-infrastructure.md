---

copyright:
  years: 2020
lastupdated: "2020-06-26"

keywords: SAP NetWeaver

subcollection: sap-vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Accessing your infrastructure
{: #access-infrastructure}

{{site.data.keyword.cloud}} offers several ways to securely connect to your infrastructure. You can establish connections on various protocols and ports by using various methods. Some options include
  * Floating IPs on the public internet
  * VPC VPN Gateway (by using IPSecVPN tunnel) deployed on the connecting site
  * {{site.data.keyword.BluDirectLink}} private connection to existing on-premises networks

To begin setup of the virtual server for SAP NetWeaver, you can establish a Secure Shell (SSH) connection by using these methods. For more information about connections, see [Connecting to your Linux Virtual Server instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux).
{: shortdesc}

## VPC VPN Gateway
{: #vpc-vpn-gateway}

To connect to a virtual server on VPC through a secure IPSec tunnel, a VPN Gateway is created for the VPC. For a tutorial that describes the setup of connectivity to the VPC VPN Gatewayusing the open source `strongSwan` IPSec-based VPN client on an external network, refer to [Use a VPC/VPN gateway for secure and private on-premises access to cloud resources](/docs/solution-tutorials?topic=solution-tutorials-vpc-site2site-vpn).

## {{site.data.keyword.cloud_notm}} Direct Link
 {: #direct-link}

Network back-bone infrastructure of a customer site can be directly connected to {{site.data.keyword.cloud_notm}}, by using {{site.data.keyword.cloud_notm}} Direct Link. On-premises resources can be connected to multiple VPCs, and VPC can provide Bring-your-own-IP or other custom IP ranges.

Technical requirements and restrictions exist in the availability of {{site.data.keyword.cloud_notm}} Direct Link in different regions. A detailed description of {{site.data.keyword.cloud_notm}} Direct Link can be found in [Getting started with IBM Cloud Direct Link](/docs/dl/getting-started?topic=dl-get-started-with-ibm-cloud-dl).
{: note}

## Next Steps
{: #next-steps-accessing-infrastructure}

  * [Downloading and installing SAP software and applications](/docs/sap-vpc?topic=sap-vpc-install_sap)

---

copyright:
  years: 2020
lastupdated: "2020-06-08"

keywords: SAP NetWeaver, {{site.data.keyword.cloud_notm}}, {{site.data.keyword.Db2_on_Cloud_short}}, FAQs, VLAN, application server, SAP Certified, high availability, highly available

subcollection: sap-vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# FAQs: SAP on IBM Virtual Private Cloud
{: #faqs}

## Do I need IBM Db2 to run SAP NetWeaver on {{site.data.keyword.cloud_notm}}?
{: faq}

For information about IBM Db2, see [SAP Note 2927211](https://launchpad.support.sap.com/#/notes/2927211){: external} and **SAP on IBM Db2 for Linux, UNIX, and Windows** on the [SAP Community page](https://community.sap.com/topics/db2-for-linux-unix-windows){: external}.

Be aware of the provided operating system in the SAP Note because different sets of service packs are required by your database.
{: note}

## Why was IBM Db2 chosen for certification for {{site.data.keyword.vpc_short}}?
{: faq}

IBM Db2 is an {{site.data.keyword.IBM_notm}} product that is seamlessly integrated into SAP NetWeaver and SAP Business Warehouse. IBM Db2 is a mature database solution for SAP applications with many advantages, for example, reduced total cost of ownership, excellent performance, and simplified administration.

## Can I split my distributed SAP environment between different data centers?
{: faq}

For a distributed SAP installation, it is best to have all nodes in the same data center and VLAN segment. Deviation from this setup can yield latency and timeouts, which render your SAP system unresponsive.

## Can I achieve SAP high availability as defined by SAP architecture?
{: faq}

The only supported high availability architecture is the scaling of application servers. No enabled hardware is available for high availability support.

## Can I download the SAP distribution software directly from {{site.data.keyword.vpc_short}}?
{: faq}

We don't have a direct link for SAP software downloads. You need to download the distribution DVDs.

---

copyright:
  years: 2020
lastupdated: "2020-06-05"

keywords: SAP in VPC, {{site.data.keyword.cloud_notm}}, {{site.data.keyword.vpc_full}}, {{site.data.keyword.vpc_short}}, {{site.data.keyword.IBM}} Metrics Collector for SAP, IMCS, {{site.data.keyword.vsi_is_full}}, {{site.data.keyword.vsi_is_short}}

subcollection: sap-vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:important: .important}

# IBM Metrics Collector for SAP
{: #metrics-collector}

{{site.data.keyword.IBM}} Metrics Collector for SAP (IMCS) collects performance-related data from {{site.data.keyword.vsi_is_full}} for SAP. The collected metric data lets the SAP Support Team monitor, troubleshoot, and improve performance of business transactions. Use the following information to help with installing, configuring, and troubleshooting the {{site.data.keyword.IBM_notm}} Metrics Collector for SAP on Linux.
{: shortdesc}

## Before you begin
{: #before-you-begin-imcs}

You need to successfully create an {{site.data.keyword.vpc_full}} and {{site.data.keyword.vsi_is_short}} by using the appropriate catalog image for SAP. Please check [SAP Note 2927211](https://launchpad.support.sap.com/#/notes/2927211) to make sure that the selected operating system is supported by SAP. The Metrics Collector runs specifically on {{site.data.keyword.vsi_is_short}} to gather required SAP metrics. Figure 1 outlines the data sources that are used by {{site.data.keyword.IBM_notm}} Metrics Collector for SAP.

Figure 1. Data sources for {{site.data.keyword.IBM_notm}} Metrics Collector for SAP

![Figure 1. Data sources for {{site.data.keyword.IBM_notm}} Metrics Collector for SAP](/images/data-sources.svg "Data sources for IBM Metrics Collector for SAP")

## Getting an {{site.data.keyword.cloud_notm}} API key
{: #get-api-key}

You need an {{site.data.keyword.cloud_notm}} API key for IMCS to successfully collect all required metrics. The API key grants view access to {{site.data.keyword.cloud_notm}} infrastructure services. You can install IMCS without an API key, however, some metrics are missing and therefore the VSI will not be supported by SAP.

You only need to create one API Key per Account. You can use the same API Key for all the Metric Collector installed in the VSI associated to the Account.
{: note}

For a list of missing metrics, see [Additional Information](#additional-information).

### Creating a Service ID
{: #service-id}

You need to first create a Service ID to get an API key. Use the following steps to create a Service ID.

You only need one service ID per account.
{:important}

1. Sign in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){: external} and click **Manage** > **Access (IAM)**.

   Figure 2. Open Access (IAM)

   ![Figure 2. Open Access (IAM)](/images/access-iam.png "Open Access (IAM)")

1. Click **Services IDs** > **Create**.
1. Enter a **Name** and **Description** for the Service ID and click **Create**. You can assign the Access Policy once your Service ID is created.
1. Click **Access Policy** > **Assign Access**.
1. Click **IAM Services** for **Assign Service ID additional access**.
1. Select **Infrastructure service** for **What type of access do you want to assign?**
1. Select **All resource groups** for **in**
1. Click **Viewer** for **Platform Access**.
1. Click **Add** > **Assign**. The Infrastructure Service policy has been assigned to your Service ID.

### Creating an API Key for the Service ID.
{: #api-key}

Use the following steps to create an API Key for the new Service ID.

1. Select **Service IDs** and verify Infrastructure Service is listed as an Access Policy. If not repeat steps 4-9.
1. Click **API keys** > **Create**.
1. Enter a **Name** and **Description** for the key and click **Create**.
1. Click **Copy** or **Download** your key.

## Installing the {{site.data.keyword.IBM_notm}} Metrics Collector for SAP on Linux
{: #install-imcs}

The {{site.data.keyword.IBM_notm}} Metrics Collector for SAP is a daemon or service that automatically starts when it's completely installed and requires an API key. It collects metrics from a virtual server's metadata, {{site.data.keyword.cloud_notm}} infrastructure services, runtime data about resources, such as CPU, memory, network, and disk. The metrics are aggregated and exposed through the web server for SAP customers. SAPOSCOL consumes the XML output of this web server.

The {{site.data.keyword.IBM_notm}} Metrics Collector for SAP uses port 18181 to expose the metrics. Make sure that `port 18181` is not used by any other application. For more information on how to check port availability, see [Troubleshooting](#troubleshooting).
{:important}

Use the following steps to download the IMCS.

1. [Download the IMCS](ftp://public.dhe.ibm.com/cloud/bluemix/sap/rhel){: external}.
1. Select the appropriate `tar.gz` file. In most cases, use the current version. Connect as **guest**.
1. Save the file to your internal Downloads folder and click **OK**.
1. Move or copy the IMCS `tar.gz` file to your VPC virtual server instance.
1. Unzip the file and open the unzipped folder.

   ```
   tar -xvf sap-metrics-collector-v1.2.tar.gz
   cd sap
   ```
   {: pre}

1. Run the `install-linux.sh` file.

   ```
   ./install-linux.sh
   ```
   {: pre}
1. Paste your [API key](#api-key) when prompted. If you don't have an API key, see [Getting an {{site.data.keyword.cloud_notm}} API key](#get-api-key).
1. Check to make sure that the `sap-metric-service` is running after the installation is complete. The service status displays `active` when it is ready.

   ```
   sudo systemctl is-active sap-metrics-collector
   ```
   {: pre}

## Verifying data collection
{: #verify-data-collection}

After the installation completes and the service is started, it can take time for the IMCS begins collecting metrics. It is recommended to wait at least two minutes after the installation before expecting full and accurate metrics.

1. Run the following `curl` command for your localhost address to see your metrics:

   ```
   [root@(where you installed ICMS)-sap ~]# curl http://localhost:18181/sap/metrics
   ```
   {: pre}

   ```
   <metrics>
       <metric category="config" context="vm" device-id="" last-refresh="refresh time" refresh-interval="0" type="string" unit="none">
           <name>Data Provider Version</name>
           <value>1.0_beta1</value>
       </metric>
       <metric category="config" context="vm" device-id="" last-refresh="refresh time" refresh-interval="0" type="string" unit="none">
           <name>Cloud Provider</name>
           <value>IBM Cloud</value>
       </metric>
   ```
   {: screen}   

   There may be a delay before your data is available.
   {:note}

## Troubleshooting
{: #troubleshooting}

Use the following troubleshooting tips for IMCS.

### Uninstalling the Metrics Collector
{: #uninstall-icmc}

1. Run the following command to uninstall IMCS if you have any issues during the installation process. Then reinstall it.

   ```
   [root@sap-mc-instl sap]# ./uninstall-linux.sh
   ```
   {: pre}

   ```
   Removing IBM Metric Collector for SAP...
   Successfully removed IBM Metric Collector for SAP.
   ```
   {: screen}

### No metrices reported when running `curl` command
{: #no-metrics}

No reported metrics is mainly due to the port not assigned to SAP Metrics Collector. It needs port `18181` available for `localhost`. If you have any other applications using the port, you have to close the applications.

1. Use the following command to see if the port is assigned to an other application.

   ```
   [root@sap-mc-redhat]# nmap -sT -O localhost
   ```
   {: pre}

   ```
   Starting Nmap 6.40 (http://nmap.org) at (date and time)
   Nmap scan report for localhost (your localhost address)
   Host is up (0.0s latency).
   Other addresses for localhost (not scanned): (localhost addresses)
   rDNS record for (localhost): sap-mc-redhat
   Not shown: (number of) closed ports
   PORT   STATE SERVICE
   (port)/tcp open  ssh
   (port)/tcp open  smtp
   Device type: general purpose
   Running: Linux 3.X
   OS CPE: cpe:/o:linux:linux_kernel:3
   OS details: Linux 3.7 3.9
   Network Distance: 0 hops
   ```
   {: screen}

### nmap not found
{: nmap-not-found}

You can install `nmap` on your system by using the appropriate package manager like `yum` or `apt-get`.

  * Command for Red Hat or Centos Linux: `yum install nmap`


## Additional information
{: addl-info}

If you don't have an {{site.data.keyword.cloud_notm}} API key, the {{site.data.keyword.IBM_notm}} Metrics Collector for SAP can't collect all of the metrics that are required by SAP, which include

  * Network Adaptor Mapping - replaced with local MAC ID.
  * Network Adaptor Bandwidth - Port Speed - defaults to 0.
  * Disk Volume Mapping - replaced with Volume Attachment ID.
  * Disk Guaranteed IOPS - defaults to 0.

You must provide an API key so that all metrics can be collected. Otherwise this VSI is not fully supported by SAP.
{:important}

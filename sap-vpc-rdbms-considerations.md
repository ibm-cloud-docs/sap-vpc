---

copyright:
  years: 2020
lastupdated: "2020-06-30"

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

# Considerations about RDBMS for SAP
{: #rdbms-considerations}

Deployment of SAP systems on {{site.data.keyword.vpc_full}} is similar to running the systems within your own infrastructure. Therefore, use the information provided from SAP and the RDBMS providers.

## Considerations about IBM Db2
{: #db2-considerations}

A good entry point into the documentation is the [SAP community page for Db2](https://community.sap.com/topics/db2-for-linux-unix-windows){: external}.

With Db2, several unique capabilities and features are available on the IBM cloud deployment as well. You can use all supported database features as mentioned in [SAP Note 1555903](https://launchpad.support.sap.com/#/notes/1555903){: external} with the exception of the IBM Db2 pureScale Feature and support for integrated cluster managers for HADR or shared disk high-availability clusters.

For details on Db2 support IaaS Cloud solutions with SAP, also refer to the [IBM Db2 knowledge center](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.5.0/com.ibm.db2.luw.licensing.doc/doc/r_suprt_db2_pblik_clouds.html){: external}.

If you install a system based on a different version of Db2, you can switch the documentation for the current release by clicking “Change version or product” in the upper left area of the IBM Knowledge Center for Db2.
{: note}

### Planning

First of all, check [SAP Note 101809](https://launchpad.support.sap.com/#/notes/101809){: external} for details about the supported Db2 versions and fix pack levels as well as [SAP Note 2927211](https://launchpad.support.sap.com/#/notes/2927211){: external} for the supported Db2 releases to be used for the deployment on {{site.data.keyword.vpc_full}}.

Beside this, follow the appropriate sections in the Installation Guides provided by SAP.

Before starting your deployment, check all relevant information and prerequisites that are mentioned in the following essential documentation:
  - [Installation Guide for SAP Products Based on SAP NetWeaver 7.1 to 7.52](https://help.sap.com/viewer/ce9e270ad34949969c16d09d1b099a26/CURRENT_VERSION/en-US/9420dabb130e4ae1996b3f39e202cc6e.html){: external}
  - [General Db2 prerequisites on Linux and UNIX](https://www.ibm.com/support/knowledgecenter/SSEPGG_11.5.0/com.ibm.db2.luw.qb.server.doc/doc/c0059823.html){: external}
  - [SAP Note 1707361 - Install Systems Based on NW 7.1 and Higher: UNIX Db2 for LUW](https://launchpad.support.sap.com/#/notes/1707361){: external}
  - [Required File systems in the SAP NetWeaver Installation Guide](https://help.sap.com/viewer/ce9e270ad34949969c16d09d1b099a26/CURRENT_VERSION/en-US/713eb64f45c6448c8dbe8a51b85680ee.html){: external}
  - [Recommended file system types for Db2](https://www.ibm.com/support/knowledgecenter/SSEPGG_11.5.0/com.ibm.db2.luw.admin.dbobj.doc/doc/r0056470.html){: external}

### Deployment

Before starting the software deployment, install the software packages for Korn Shell and Asynchronous IO on the virtual server instance. It is also helpful to install zip and unzip.
```
yum install ksh
yum install aio
yum install zip
yum install unzip
```

Also ensure that all required packages are installed as mentioned in [SAP Note 2002167](https://launchpad.support.sap.com/#/notes/2002167){: external}.

The default port for the TCP/IP communication between the Db2 server and the Db2 client when using SWPM is `5912`. However, this port is already defined. Either choose a different port during installation or remove the entry for this port number from the `/etc/services` file.
```
...
cpdlc           5911/sctp               # Controller Pilot Data Link Communication
fis             5912/tcp                # Flight Information Services
fis             5912/udp                # Flight Information Services
fis             5912/sctp               # Flight Information Services
ads-c           5913/tcp                # Automatic Dependent Surveillance
...
```

If you want to invoke the SAP installer with the `SAPINST_REMOTE_ACCESS_USER` option, you can either temporarily set a password for the root user or create an OS user for SWPM and set its password.

```
useradd swpmuser
passwd swpmuser
```

Start the installation process by using an SWPM user:

```
ssh -i ~/.ssh/sap-ssh-key root@<your IP>
cd /db6upload/swpm/ 
./sapinst SAPINST_REMOTE_ACCESS_USER=swpmuser 
```

Confirm with: y

### Sizing considerations for Db2

When you are using Db2 with SAP applications, sizing is following the general SAP sizing rules as described in the SAP Quick Sizer documentation. However, certain Db2 functions requires special considerations.

When you are using Db2 columnar organized tables, also known as IBM® Db2 BLU Acceleration the minimal recommended server configuration is at least 64 GB memory and 4 CPU cores, exclusively available for the database workload. Further information about sizing and usage of Db2 BLU Acceleration can be found in [SAP Note 1819734](https://launchpad.support.sap.com/#/notes/1819734){: external}.

### SAP Business Warehouse

When Using SAP Business Warehouse with the Db2 database partitioning feature, a stable and fast network connections need to be established between all host in the Db2 DPF cluster. For details, check the following Information: [SAP Business Warehouse on IBM Db2 for Linux, UNIX, and Windows 10.5 and Higher: Administration Tasks](https://help.sap.com/viewer/db6_bw/c289a552d161224fe10000000a445394.html){: external}.


## Considerations about SAP ASE
{: #ase-considerations}

On the SAP ASE landing page you will find useful documentation and reference manuals - see [SAP Help Portal -- SAP Adaptive Server Enterprise](https://help.sap.com/viewer/product/SAP_ASE/16.0.3.7/en-US){: external}. Another good source of information is the [SAP Applications on SAP Adaptive Server Enterprise Community Page](https://community.sap.com/topics/applications-on-ase){: external}.

### Planning

Be sure to check [SAP Note 2489781](https://launchpad.support.sap.com/#/notes/2489781){: external} for details about the supported operating systems and versions as well as [SAP Note 2927211](https://launchpad.support.sap.com/#/notes/2927211){: external} for the supported ASE releases to be used for the deployment on {{site.data.keyword.vpc_full}}.

Before starting your deployment, check all relevant information and prerequisites that are mentioned in the following essential documentation:
  - [Installation of SAP Systems Based on the Application Server ABAP of SAP NetWeaver 7.3 to 7.52 on UNIX: SAP Adaptive Server Enterprise](https://help.sap.com/doc/4f95c9e3741a1014955595407d8604de/CURRENT_VERSION/en-US/Inst_nw7x_unix_ase_abap.pdf){: external}
  - [SAP Note 1748888 - Installing Systems Based on NW 7.3 and Higher: SAP ASE](https://launchpad.support.sap.com/#/notes/1748888){: external}
  - [Minimally Configuring an SAP ASE Server](https://help.sap.com/viewer/23c3bb4a29be443ea887fa10871a30f8/16.0.3.7/en-US/a6371abfbc2b10149c66e4e41119a6da.html){: external}

 ### Deployment

As already mentioned, deployment on {{site.data.keyword.vpc_full}} similar to deployment on-premise. However, there are subtle differences in getting the required infrastructure deployed and accessing it. Refer to the steps in [Sample SAP NetWeaver deployment](/docs/sap-vpc?topic=sap-vpc-sample-sap-netweaver-deployment) and then follow the SAP installation guides and the SAP Notes listed above.

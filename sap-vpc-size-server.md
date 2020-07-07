---

copyright:
  years: 2020
lastupdated: "2020-06-25"

keywords: SAP NetWeaver, SAP Application Performance Standard, SAPS, SAP Quick Sizer

subcollection: sap-vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:table: .aria-labeledby="caption"}


# Sizing the virtual servers
{: #size_the_server}

After you decide which SAP solutions you want to use, you need to determine the number of {{site.data.keyword.vsi_is_full}} that you need to support your SAP landscape and make sure that the servers are correctly sized.
{: shortdesc}

## Understanding the SAP sizing methodology
{: #size_method}

The SAP sizing methodology for SAP NetWeaver-centric applications is based on SAP benchmarks, such as information from SAP and actual customer experiences. The base SAP workload unit is an SAP Application Performance Standard (SAPS). The SAPS is a definition of throughput that is coined by SAP capacity planning and performance testing personnel. For example, 100 SAPS are defined as 2,000 fully business processed order-line items per hour in the standard SAP Sales and Distribution (SAP SD) application benchmark. This example is equivalent to 2,400 SAP SD transactions per hour with the SAP Enterprise Resource Planning (SAP ERP) solution.

The benchmarking test rates the infrastructure processing power on its capability to run and process transactions at close to 100% CPU load with a response time of less than 1 second.

The capability of processors is measured during the standard (SAP SD) benchmark test that is certified by SAP. For more information about the benchmark test that is certified by SAP, see [SAP Standard Application Benchmarks](https://www.sap.com/about/benchmark/measuring.html){: external} and [Practical Guidelines and Techniques for Sizing your SAP Landscape for Optimal Performance and Scalability](https://www.sap.com/documents/2016/10/c2206376-8f7c-0010-82c7-eda71af511fa.html){: external}.

## Using the SAP Quick Sizer
{: #quicksizer}

The [SAP Quick Sizer](https://service.sap.com/quicksizer){: external} is a web-based tool that is provided by SAP. Sizing information is input directly into the tool and sizes SAP NetWeaver-based applications and non-SAP NetWeaver-based applications.

You need an SAP S-user ID to use the Quick Sizer.
{: note}

The Quick Sizer calculates the workload (in SAPS) and adjusts the workload to allow for suitable processor use. So, if a workload of 4,800 SAP SD benchmark transactions per hour is required, the tool calculates this workload as 200 SAPS. An example calculation is an IT department has determined that to avoid system overloads during high-usage periods, a target processor load of 33% is allowed. A processor that can provide 200 SAPS at 33% load means the processor is capable of 600 SAPS at 100% load. Therefore, a system capable of 600 SAPS becomes the benchmark to which any new infrastructure must adhere.

While the sizing method might be considered conservative, consider that all SAPS calculations for your servers are based on highly tuned SAP systems that run only specific SAP SD workloads. Depending on the type of SAP application, any custom configuration or custom coding in your system, your results can vary. Also, requirements for your project, such as proof-of-concept (PoC) or regarding performance and response time, might be different.

## Choosing an SAP-certified IBM Cloud™ Virtual Server for Virtual Private Cloud
{: #choose_server}

After you determine your SAP applications and the SAPS numbers are calculated through the SAP Quick Sizer, or based on your current landscape, you can choose {{site.data.keyword.vsi_is_short}} from the offered profiles. Table 1 contains the SAPS numbers for the {{site.data.keyword.vsi_is_short}} that are certified for SAP NetWeaver.

All supported {{site.data.keyword.vsi_is_short}} profiles that are listed in Table 1 match the profiles that are certified by SAP with the published name and are verified in [SAP Note 2927211](https://launchpad.support.sap.com/#/notes/2927211){: external}.

The published names are subject to change.
{: note}

| Virtual Server | vCPU | Memory (GB) | SAPS |
| --- | --- | --- | --- |
| **Balanced** | | | |
| bx2-2x8 | 2 | 8 | 2,300 |
| bx2-4x16 | 4 | 16 |  4,600 |
| bx2-8x32 | 8 | 32 |  9,200 |
| bx2-16x64 | 16 | 64 | 18,450 |
| bx2-32x128 | 32 | 128 | 36,900 |
| bx2-48x192 | 48 | 192 | 55,350 |
| **Memory optimized** | | | |
| mx2-2x16 | 2 | 16 | 2,570 |
| mx2-4x32 | 4 | 32 | 5,140 |
| mx2-8x64 | 8 | 64 | 10,280 |
| mx2-16x128 | 16 | 128 | 20,560 |
| mx2-32x256 | 32 | 256 | 41,130 |
| mx2-48x384 | 48 | 384 | 56,970 |
{: caption="Table 1. {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} certified for SAP NetWeaver" caption-side="top"}

## Migrating an existing SAP system
{: #migrating}

If you plan to migrate an existing SAP system from any source to your {{site.data.keyword.vpc_short}} environment, you can determine the SAPS numbers from the SAPS numbers of your current environment.

Use the information about your current workload (the CPUs and RAM used) and get the SAPS equivalents for your CPU from the [SAP SD Benchmarks](https://www.sap.com/about/benchmark.html){: external} of your existing hardware.

## Next steps
{: #size-server-next-steps}

  * [Determining your configuration](/docs/sap-vpc?topic=sap-vpc-determine_configuration)

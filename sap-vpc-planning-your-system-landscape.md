---

copyright:
  years: 2020
lastupdated: "2020-05-29"

keywords: SAP NetWeaver, SAP landscape, SAP Business Suite, SAP Business Warehouse, SAP BW

subcollection: sap-vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:faq: .faq}

# Planning your SAP system landscape
{: #planning-your-system-landscape}

An SAP landscape is a group of two or more SAP systems that usually include development, quality and test, and production. One SAP system consists of one or more *SAP instances*, which are a group of processes that are started and stopped at the same time. These *SAP instances* are grouped to form a specified SAP system for a defined use for a region or business unit. Then, the instances are grouped in a landscape as development, test, or production systems. Each system includes one or multiple tracks (such as "project" and "business"). This landscape design is up to each business, dependent on business requirements.
{: shortdesc}

Landscapes have several possible configurations, such as server (CPU, RAM) size and storage size, for all SAP solutions in the market. These solutions include SAP NetWeaver-based products. SAP NetWeaver-based products range from older solutions, such as SAP ECC and SAP BW (that use "AnyDB" vendors that are approved by SAP), to the new range of SAP solutions, such as SAP S/4HANA and SAP BW/4HANA (that use SAP HANA database). Beyond the enterprise resource planning (ERP) and enterprise data warehouse (EDW) examples, there are many available SAP products or add-ons for different industries and business types.

SAP NetWeaver-based products are designed to run on SAP NetWeaver-certified {{site.data.keyword.virtualmachinesshort}}. The certified {{site.data.keyword.vsi_is_short}} operating systems and supported database systems for {{site.data.keyword.cloud}} are listed in [SAP Note 2927211](https://launchpad.support.sap.com/#/notes/2927211).

More solutions are available from SAP that are not SAP NetWeaver-based and many third-party software options that might integrate with SAP can affect the planning of your system landscape. Contact [SAP Support](https://support.sap.com/en/index.html){: external} if you plan to deploy and integrate non-SAP NetWeaver-based or third-party software into your SAP landscape on {{site.data.keyword.cloud_notm}}.
{: note}

You want to be as detailed as possible when you determine the size of your server based on the applications that you plan to run, potential growth, and performance. Additionally, keep in mind your storage and memory requirements for your applications. SAP systems in a landscape have specific requirements for connectivity, either among each other or to external systems. For more information, see [Items to consider when you plan your SAP landscape](/docs/sap-vpc?topic=sap-vpc-considerations).

## Necessary account credentials for SAP and IBM Cloud
{: #get_sap_ibm_credentials}

Use the following steps to obtain account credentials for SAP and {{site.data.keyword.cloud_notm}}.

### SAP credentials and accounts for new users
{: #sap_credentials}

1. To set up an SAP ID account, follow the instructions on the [SAP Support Portal Home](https://support.sap.com/en/my-support/users.html){: external}.

### {{site.data.keyword.cloud_notm}} credentials and accounts for new users
{: #ibm_credentials}

1. Go to [Getting started with {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/get-started){: external} and click **Start for free** in the upper right corner.

   After you have entered your email address an email is sent from {{site.data.keyword.cloud_notm}} Support that contains your verification code, from which you can create your initial login ID and password.

1. Set up your [profile](/docs/account?topic=account-usersettings#profile-photo) to log in as a new user.

For more information, see [Signing up for {{site.data.keyword.cloud_notm}}](docs/account?topic=account-signup).

After you receive your login credentials and have access to your {{site.data.keyword.cloud_notm}} account, you can create other accounts and users. See [Best practices for assigning access](/docs/iam?topic=iam-account_setup) to learn more about assigning access to {{site.data.keyword.cloud_notm}}.

## Reviewing any relevant SAP and IBM Cloud documentation
{: #review_doc}

Review the following documentation to help you determine any prerequisites for the SAP products that you plan to install on your {{site.data.keyword.vsi_is_full}}.

If your organization is new to {{site.data.keyword.cloud}}, read the following SAP documentation to help with your planning phase.
  * [How to create an SAP S-user ID](https://www.youtube.com/watch?v=4wICiRTP8u0/){: external} Note that only super administrators or S-users with the required authorization are allowed to create S-user IDs.
  * Applicable [installation guides](https://support.sap.com/en/my-support/software-downloads.html){: external}; requires an SAP S-user ID.
  * SAP release notes, which can be found in the application help of the relevant SAP product documentation on the [SAP Help Portal](https://help.sap.com/viewer/index){: external}; requires an SAP S-user ID.
  * [SAP NetWeaver Help](https://help.sap.com/viewer/product/SAP_NETWEAVER/ALL/en-US){: external}
  * [SAP Product Availability Matrix (PAM)](https://support.sap.com/en/release-upgrade-maintenance.html#section_1969201630){: external}; requires an SAP S-user ID.
  * [SAP Notes](https://support.sap.com/en/my-support/knowledge-base.html){: external}; requires an SAP S-user ID.
  * Third-party documentation

## Determining your SAP NetWeaver applications
{: determine-apps}

Your business and functional requirements determine the SAP solutions to run on the SAP NetWeaver stack. Your requirements have an influence on how you size your server. You have a wide selection of SAP NetWeaver-based applications to choose from, including SAP ERP Central Component (ECC), SAP S/4HANA, and customer engagement and commerce solutions.

Items to consider when you make your determinations:
  * How do you intend to use the applications? Is the intended use for development and testing, or production?
  * How do you intend to connect your SAP workloads in Cloud to your existing network and systems?

## Next Steps
{: #planning-landscape-next-steps}

Use the following to help your planning process and to help build your target SAP landscape.

  * [Consideration when planning your SAP landscape](/docs/sap-vpc?topic=sap-vpc-considerations)
  * [Size the server](/docs/sap-vpc?topic=sap-vpc-size_the_server#size_the_server)
  * [Determine your configuration](/docs/sap-vpc?topic=sap-vpc-determine_configuration#determine_configuration)

Select the appropriate option to begin planning your SAP landscape.

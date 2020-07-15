---

copyright:
  years: 2020
lastupdated: "2020-06-26"

keywords: SAP NetWeaver, {{site.data.keyword.vpc_full}}, {{site.data.keyword.vpc_short}}

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

# Setting up your IBM Cloud account structure
{: #account-structure}


The {{site.data.keyword.cloud}} console is the UI for an {{site.data.keyword.cloud_notm}} account and requires a unique log-in ID, which is an {{site.data.keyword.IBM_notm}}id. You can add extra {{site.data.keyword.IBM_notm}}id's and users by using Identity and Access Managment (IAM) to control privileges such as ordering compute resources. Use the steps within the sections below to set up your {{site.data.keyword.cloud_notm}} account structure to establish the best functionality for your SAP landscape.
{: shortdesc}

Use resource groups to organize your account resources for access control and billing purposes. {{site.data.keyword.cloud}} provides all users with a default resource group. Before you consider creating new resource groups, become familiar with this topic by reading [Best practices for organizing resources in a resource group](/docs/resources?topic=resources-bp_resourcegroups).

## Creating Resource Groups
{: #resource-groups}

1. Log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){: external} with your unique credentials.
1. Click **Manage** > **Account** > **Account Resources > Resource Groups**.
1. Click **Create**, enter a unique **Name** for the new Resource Group, and click **Create**.

For more information about resource groups, see [Managing resource groups](/docs/resources?topic=resources-rgs).

## Defining Cloud Identity and Access Management
{: #iam}

Optional setup of IAM controls.
{: note}

Significant granularity of access control is available for all {{site.data.keyword.cloud_notm}} services for specified Resource Groups through IAM Access policies. For more information about IAM roles and actions, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started). An account owner or administrator is assumed to have performed IAM setup for the account, including assignment of all authorizations required to provision infrastructure. If the actions described in this document are being performed by another user, then you need to check the current IAM authorizations to complete the tasks for deploying and running SAP NetWeaver on {{site.data.keyword.vpc_full}}.

### Checking current IAM authorizations
{: #check-current-iam}

Managing your {{site.data.keyword.cloud_notm}} environment and preparing it to run SAP NetWeaver includes ordering your block storage, securing the virtual server, downloading and installing your SAP NetWeaver software and applications, and testing connectivity to your {{site.data.keyword.vpc_short}} environment. Confirm with an administrator that you have the permissions to check current authorizations. For more information about the required roles and permissions, see [Granting user permissions for VPC resources](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources) and make sure that you have the settings for the **Full-access scenario**.

Use the following steps to check the current authorizations.

1. Select **Manage** > **Access (IAM)**.
1. Click **Users** and select your user name. Names are in first name alphabetical order.
1. Click **Access Policies** and click on the number next to the **Role** to see the currently assigned roles and the allowed actions.
1. Click **Roles**.
1. Click **'View the roles for'** and select **{{site.data.keyword.vsi_is_short}}**. This selection shows the available Roles and allowed actions for a {{site.data.keyword.vsi_is_short}}. An Administrator can apply these roles to you or alter them to provide authorization for the necessary actions.

For more information about IAM, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started).

## Accessing the classic infrastructure
{: #classic-access}

Optional setup.
{: note}

{{site.data.keyword.vpc_short}} infrastructure for SAP NetWeaver can access other resources on {{site.data.keyword.cloud_notm}} Classic Infrastructure, such as high-performance {{site.data.keyword.baremetal_long}}. You have multiple options to achieve this access, notably a one-to-one association, or {{site.data.keyword.tg_full}} with increased flexibility. All options require upgrading the {{site.data.keyword.cloud_notm}} account to be VRF-enabled. For more information on VPC access to Classic Infrastructure, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure). For more information on {{site.data.keyword.tg_short}}, see   [Getting started with {{site.data.keyword.tg_full_notm}}](/docs/transit-gateway?topic=transit-gateway-getting-started).

## Setting up a Virtual Private Cloud (VPC) and subnet
{: #establish-vpc}

1. Click **Menu icon** ![Menu icon](../../icons/icon_hamburger.svg) > **VPC Infrastructure** > **Network** > **VPCs** and click **New virtual private cloud**.

  SAP-certified Virtual Servers are available on the Generation 2 compute environment.
  {: note}

1. Click **Create VPC for Gen 2**.
1. Enter a unique **Name** for the VPC.
1. Select a **Resource group**. Use resource groups to organize your account resources for access control and billing purposes. For more information, see [Best practices for organizing resources in a resource group](/docs/resources?topic=resources-bp_resourcegroups).
1. _Optional:_ Enter tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/resources?topic=resources-tag).
1. Select whether the **Default security group** allows inbound SSH and ping traffic to virtual server instances in this VPC. We'll configure more rules for the default security group later.
1. _Optional: Classic access_. Select whether you want to enable your VPC to access classic infrastructure resources. For more information, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure).

    You can only enable a VPC for classic access while creating it. In addition, you can only have one classic access VPC in your account at any time.
    {:important}

1. _Optional: Default address prefixes_. Disable this option if you don't want to assign default subnet address prefixes to each zone in your VPC. After you create your VPC, you can go to its details page and set your own subnet address prefixes. If you do disable this option, the **New subnet for VPC** section will be hidden, and will require manual definition after the VPC is created. Leave the value default.

### New subnet for VPC
{: #new-subnet-for-vpc-account}

1. Enter a unique **Name** for the VPC subnet.
1. Select a **Resource group** for the subnet.
1. Select a **Location** for the subnet. The location consists of a region and a zone.

    The region that you select is used as the region of the VPC. All additional resources that you create in this VPC are created in the selected region.
    {: tip}

1. Enter an **Address prefix**, **Number of addresses**, and an **IP range** for the subnet. The IP range is entered in CIDR notation, for example: `10.240.0.0/24`. In most cases, you can use the default IP range. If you want to specify a custom IP range, you can use the IP range calculator to select a different address prefix or change the number of addresses.

    A subnet cannot be resized after it has been created.
    {:important}

1. Attach a public gateway to the subnet if you want to allow all attached resources to communicate with the public internet.  

    You can also attach the public gateway after you create the subnet.
    {: tip}

1. Click **Create virtual private cloud**.

## Manually defining your subnets
{: #manually-defining-vpc-subnets}

If you disabled _Default address prefixes_, which hid the _New subnet for VPC_ section on the VPC ordering page, you need to manually define your subnets prior to provisioning your {{site.data.keyword.vsi_is_short}}. Use the steps below to set up your subnets if you didn't do so when you set up your VPC.

1. Click **Menu icon** ![Menu icon](../../icons/icon_hamburger.svg) > **VPC Infrastructure** > **Subnets** > **New Subnet**.
1. Enter a unique **Name** and select the **Virtual private cloud** that it's to be associated with.
1. Select a **Resource group**.
1. Select **Location**. The location consists of a region and zone. If you're manually creating the initial subnet, versus creating it when you created your VPC, the region (location) that you select is used as the region of the VPC. All additional resources that you in this VPC are created in the selected region.

   If you created a subnet when you created your VPC, additional subnets must be created in the same region. You can create multiple subnets within VPC zones.

1. Click **Create subnet**.

## Next Steps
{: #account-structure-next-steps}

  * [Setting up your infrastructure](/docs/sap-vpc?topic=sap-vpc-set_up_infrastructure)
  * [Accessing your infrastructure](/docs/sap-vpc?topic=sap-vpc-access-infrastructure)

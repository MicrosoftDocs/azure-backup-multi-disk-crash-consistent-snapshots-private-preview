---
title: Azure Backup support for multi-disk crash-consistent snapshots (private preview)
description: Learn about the configuration of Enhanced policy to capture multi-disk crash-consistent recovery points.
ms.topic: how-to
ms.service: backup
ms:date: 08/10/2023
author: AbhishekMallick-MS
ms.author: v-abhmallick
---

# Azure Backup support for multi-disk crash-consistent snapshots (private preview)

Azure Backup now allows you to capture multi-disk crash-consistent recovery points if you have opted for crash-consistent snapshots in Backup Policy or in case application-consistent/file-consistent recovery points generation fails due to some errors.  

To enroll your subscription for this private preview feature, fill [this form](https://aka.ms/AzureBackupSupportForMultiDiskCrashConsistencyForm).

***Important***

*The **regions** text box in the enrollment form is only to help the Azure Backup team to understand priority order of the regions for enabling this feature only. However, once your subscription is enrolled, this private preview feature will be available for the subscription in all regions where this feature is supported. This also implies that if Azure Backup enables this feature in new regions, the enrolled subscription will also have this feature available in the new regions.*

## Regions availability

This feature is currently deployed in *all Azure public regions*. However, to use the feature, subscription allow-listing is required.

## Support matrix 

- All Azure Public regions are supported.
- Multi-disk crash-consistency is supported only with [Enhanced Policy](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Fbackup%2Fbackup-azure-vms-enhanced-policy%3Ftabs%3Dazure-portal&data=05%7C01%7Cshsangal%40microsoft.com%7Cf79ec5af57d347ad240608db9402d03c%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C638266512130715342%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C3000%7C%7C%7C&sdata=Hqy9bGGKCZayuTLWiqh2nMAR6vFr6G1a2V8tOZ%2FVsxc%3D&reserved=0). 
- Trusted Launch VMs are supported.
- Managed-disks of size *4 TB* and above (striped disks) aren't supported.
- Managed-disks with paid bursting (striped disks) aren't supported.
- Ultra-disks, Premium v2 SSD, Ephemeral OS disks, Shared disks, and Write Accelerated disks aren't supported.


***Note***

*For disks configured with read/write host caching, multi-disk crash consistency may not function because writes occurring while the snapshot is taken may not be considered by Azure Storage. If maintaining consistency is crucial, we recommend you not to opt for this private preview and use the [default behavior](https://learn.microsoft.com/en-us/azure/backup/backup-azure-vms-introduction#snapshot-consistency).*

## Configure crash-consistency

Follow these steps:

1. Once your subscription is enrolled for this feature, create a new (or modify an existing) Backup Policy of type **Enhanced Policy**, and then select **Only crash consistent snapshot (Private Preview)** as **Consistency Type**.

   ***Note***

   *This feature is only available with Enhanced Policy and if your subscription is enrolled for private preview.*

   ![Screenshot shows how to enable crash consistency in Enhanced Policy for Azure VM backup.](https://github.com/MicrosoftDocs/azure-backup-multi-disk-crash-consistent-snapshots-private-preview/blob/main/articles/media/enable-crash-consistency-in-enhanced-policy.png)

2. Enable [Azure Backup on the VM using this Enhanced Policy](https://learn.microsoft.com/en-us/azure/backup/backup-azure-vms-enhanced-policy?tabs=azure-portal) if not already enabled. Azure VMs with this policy will take crash-consistent snapshots only.

***Note***

- *If you protect a VM using the above policy in any of the regions where Azure Backup has deployed this feature, VM will have crash-consistent snapshots only. Even if Azure Backup deploys this feature in new regions and you have a VM protected using the above Backup Policy in any of those regions, it'll also have crash-consistent snapshots only.*
- *If you protect a VM using the above policy in any region where Azure Backup hasn't deployed this feature, it will continue with the [default behavior](https://learn.microsoft.com/en-us/azure/backup/backup-azure-vms-introduction#snapshot-consistency).*
- *VMs in regions where this feature is enabled by Azure Backup and which are in the enrolled subscription and protected using Enhanced Policy (but not with a policy created with option ***Only crash-consistent snapshot (private preview)*** as mentioned above) will also have crash-consistent snapshots generated if application-consistent/ file-consistent snapshot generation fails due to some issues.*


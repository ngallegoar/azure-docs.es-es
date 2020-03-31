---
title: Matriz de módulo y versión de Ansible para Azure | Microsoft Docs
description: Matriz de módulo y versión de Ansible para Azure
keywords: ansible, roles, matriz, versión, azure, devops
ms.topic: reference
ms.date: 10/14/2019
ms.openlocfilehash: 54e27c7570ba1cdbce7355740181d68a2f3efbac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74155992"
---
# <a name="ansible-module-and-version-matrix"></a>Matriz de módulo y versión de Ansible

Ansible incluye un conjunto de módulos para su uso en el aprovisionamiento y configuración de los recursos de Azure. Estos recursos incluyen máquinas virtuales, conjuntos de escalado, servicios de redes y servicios de contenedores. En este artículo se enumeran los distintos módulos de Ansible para Azure y las versiones de Ansible en las que se incluyen.

## <a name="ansible-modules-for-azure"></a>Módulos de Ansible para Azure

Los siguientes módulos se pueden ejecutar directamente en hosts remotos o mediante cuadernos de estrategias.  

Estos módulos están disponibles en la versión oficial de Ansible y en los siguientes roles de cuaderno de estrategias de Microsoft.

> [!NOTE]
> Desde Ansible 2.9 en adelante, hemos cambiado el nombre de todos los módulos *_facts a *_info para seguir la convención de nomenclatura de Ansible. Los módulos antiguos y los renombrados se vinculan de forma que, aparte de mostrarse una advertencia de desuso, todos los módulos funcionen como antes.

| Módulo Ansible para Azure                   |  Ansible 2.4 |  Ansible 2.5 |  Ansible 2.6 | Ansible 2.7 | Ansible 2.8 | Ansible 2.9 | Rol de Ansible | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------|--------------|--------------|--------------|  
| **Proceso**                    |           |                          |                          |                            |           |           |           |
| azure_rm_availabilityset                   | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_availabilityset_info              | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_batchaccount                       | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_deployment                         | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_deployment_info                   | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_functionapp                        | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_functionapp_info                  | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_gallery                            | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_gallery_info                       | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_galleryimage                       | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_galleryimage_info                  | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_galleryimageversion                | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_galleryimageversion_info           | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_image                              | -            | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_image_info                        | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_resource                           | -            | -                           | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_resource_info                     | -            | -                           | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_resourcegroup                      | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_resourcegroup_info                | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_snapshot                           | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_virtualmachine                     | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualmachine_info               | -            | -                           | -            | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualmachineextension            | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualmachineextension_info      | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_virtualmachineimage_info          | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualmachinescaleset             | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualmachinescaleset_info       | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualmachinescalesetextension    | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_virtualmachinescalesetextension_info | -            | -                        | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_virtualmachinescalesetinstance     | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_virtualmachinescalesetinstance_info | -            | -                         | -            | -            | Sí          | Sí          | Sí          |
| **Redes**                              |              |                             |              |              |              |              |              |
| azure_rm_appgateway                         | -            | -                           | -            | Sí          | Sí          | Sí          | Sí          |
| azure_rm_applicationsecuritygroup           | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_applicationsecuritygroup_info     | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_cdnendpoint                        | -            | -                         | -          | -            | Sí          | Sí          | Sí          |
| azure_rm_cdnendpoint_info                  | -            | -                         | -          | -            | Sí          | Sí          | Sí          |
| azure_rm_cdnprofile                         | -            | -                         | -          | -            | Sí          | Sí          | Sí          |
| azure_rm_cdnprofile_info                   | -            | -                         | -          | -            | Sí          | Sí          | Sí          |
| azure_rm_dnsrecordset                       | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_dnsrecordset_info                 | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_dnszone                            | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_dnszone_info                      | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_firewall                           | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_firewall_info                      | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_loadbalancer                       | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_loadbalancer_info                 | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_networkinterface                   | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_networkinterface_info             | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_publicipaddress                    | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_publicipaddress_info              | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_route                              | -            | -                           | -            | Sí          | Sí          | Sí          | Sí          |
| azure_rm_routetable                         | -            | -                           | -            | Sí          | Sí          | Sí          | Sí          |
| azure_rm_routetable_info                   | -            | -                           | -            | Sí          | Sí          | Sí          | Sí          |
| azure_rm_securitygroup                      | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_securitygroup_info                 | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_subnet                             | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_subnet_info                       | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_trafficmanagerendpoint             | -            | -                         | -          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_trafficmanagerendpoint_info       | -            | -                         | -          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_trafficmanagerprofile              | -            | -                         | -          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_trafficmanagerprofile_info        | -            | -                         | -          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualnetwork                     | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualnetwork_info               | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualnetworkgateway              | -            | -                         | -          | -            | Sí          | Sí          | Sí          |
| azure_rm_virtualnetworkpeering              | -            | -                         | -          | -            | Sí          | Sí          | Sí          |
| azure_rm_virtualnetworkpeering_info         | -            | -                         | -          | -            | -            | Sí          | Sí          |
| **Storage**                    |           |                          |                          |                            |           |           |         |
| azure_rm_manageddisk                        | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_manageddisk_info                  | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_storageaccount                     | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_storageaccount_info               | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_storageblob                        | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| **Web**                    |           |                          |                          |                             |           |           |           |
| azure_rm_appserviceplan                     | -            | -                         | -          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_appserviceplan_info               | -            | -                         | -          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_webapp                             | -            | -                         | -          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_webapp_info                       | -            | -                         | -          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_webappslot                         | -            | -                         | -          | -            | Sí          | Sí          | Sí          |
| **Contenedores**                    |           |                          |                          |                            |           |           |          |
| azure_rm_acs                                | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_aks                                | -            | -                           | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_aks_info                          | -            | -                           | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_aksversion_info                   | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_containerinstance                  | -            | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_containerinstance_info            | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_containerregistry                  | -            | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_containerregistry_info            | -            | -                           | -            | Sí          | Sí          | Sí          | Sí          |
| azure_rm_containerregistryreplication       | -            | -                           | -            | -            | -          | -          | Sí          |
| azure_rm_containerregistryreplication_info  | -            | -                           | -            | -            | -          | -          | Sí          |
| azure_rm_containerregistrywebhook           | -            | -                           | -            | -            | -          | -          | Sí          |
| azure_rm_containerregistrywebhook_info      | -            | -                           | -            | -            | -          | -          | Sí          |
| **Bases de datos**                    |           |                          |                          |                             |           |           |          |
| azure_rm_cosmosdbaccount                    | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_cosmosdbaccount_info              | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_mariadbconfiguration               | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_mariadbconfiguration_info         | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_mariadbdatabase                    | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_mariadbdatabase_info              | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_mariadbfirewallrule                | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_mariadbfirewallrule_info          | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_mariadbserver                      | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_mariadbserver_info                | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_mysqlconfiguration_info           | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_mysqldatabase                      | -            | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_mysqldatabase_info                | -            | -                           | -            | Sí          | Sí          | Sí          | Sí          |
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_mysqlfirewallrule_info            | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_mysqlserver                        | -            | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_mysqlserver_info                  | -            | -                           | -            | Sí          | Sí          | Sí          | Sí          |
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_postgresqlconfiguration_info      | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_postgresqldatabase                 | -            | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_postgresqldatabase_info           | -            | -                           | -            | Sí          | Sí          | Sí          | Sí          |
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_postgresqlfirewallrule_info       | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_postgresqlserver                   | -            | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_postgresqlserver_info             | -            | -                           | -            | Sí          | Sí          | Sí          | Sí          |
| azure_rm_rediscache                         | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_rediscache_info                   | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_rediscachefirewallrule             | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_sqldatabase                        | -            | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_sqldatabase_info                  | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_sqlelasticpool                    | -            | -                           | -            | -            | -          | -          | Sí          |
| azure_rm_sqlelasticpool_info               | -            | -                           | -            | -            | -          | -          | Sí          |
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | Sí          | Sí          | Sí          | Sí          |
| azure_rm_sqlfirewallrule_info              | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_sqlserver                          | -            | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_sqlserver_info                    | -            | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| **Analytics**                    |           |                          |                          |                             |           |           |          |
| azure_rm_hdinsightcluster                   | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_hdinsightcluster_info              | -            | -                           | -            | -            | -            | Sí          | Sí          |
| **Integración**                    |           |                          |                          |                             |           |           |          |
| azure_rm_servicebus                         | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_servicebus_info                   | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_servicebusqueue                    | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_servicebussaspolicy                | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_servicebustopic                    | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_servicebustopicsubscription        | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| **Seguridad**                    |           |                          |                          |                             |           |           |           |
| azure_rm_keyvault                           | -            | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_keyvault_info                     | -            | -                           | -              | -          | Sí          | Sí          | Sí          |
| azure_rm_keyvaultkey                        | -            | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_keyvaultkey_info                   | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_keyvaultsecret                     | -            | Sí                         | Sí          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_roleassignment                     | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_roleassignment_info               | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_roledefinition                     | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_roledefinition_info               | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| **DevOps**               |           |                          |                          |                             |           |           |                  |
| azure_rm_devtestlab                         | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_devtestlab_info                   | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_devtestlabarmtemplate_info        | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_devtestlabartifact_info           | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_devtestlabartifactsource           | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_devtestlabartifactsource_info     | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_devtestlabcustomimage              | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_devtestlabcustomimage_info         | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabenvironment              | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_devtestlabenvironment_info         | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabpolicy                   | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_devtestlabpolicy_info              | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabschedule                 | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_devtestlabschedule_info            | -            | -                           | -            | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabvirtualmachine           | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_devtestlabvirtualmachine_info | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_devtestlabvirtualnetwork           | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_devtestlabvirtualnetwork_info     | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| **Azure Monitor**                           |              |                           |            |              |                 |           |              |
| azure_rm_autoscale                          | -            | -                         | -          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_autoscale_info                    | -            | -                         | -          | Sí          | Sí          | Sí          | Sí          |
| azure_rm_loganalyticsworkspace              | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_loganalyticsworkspace_info        | -            | -                           | -            | -            | Sí          | Sí          | Sí          |
| azure_rm_monitorlogprofile                  | -            | -                           | -            | -            | -            | Sí          | Sí          |
| **Administración y gobernanza**     |              |                           |            |            |            |            |              |
| azure_rm_automationaccount        | -            | -                         | -          | -          | -          | Sí        | Sí          |
| azure_rm_automationaccount_info   | -            | -                         | -          | -          | -          | Sí        | Sí          |
| azure_rm_lock                     | -            | -                         | -          | -          | -          | Sí        | Sí          |
| azure_rm_lock_info                | -            | -                         | -          | -          | -          | Sí        | Sí          |
| **Internet de las cosas**            |              |                           |            |            |            |            |              |
| azure_rm_iotdevice                | -            | -                         | -          | -          | -          | Sí        | Sí          |
| azure_rm_iotdevice_info           | -            | -                         | -          | -          | -          | Sí        | Sí          |
| azure_rm_iotdevicemodule          | -            | -                         | -          | -          | -          | Sí        | Sí          |
| azure_rm_iothub_info              | -            | -                         | -          | -          | -          | Sí        | Sí          |
| azure_rm_iothub_info              | -            | -                         | -          | -          | -          | Sí        | Sí          |
| azure_rm_iothubconsumergroup      | -            | -                         | -          | -          | -          | Sí        | Sí          |

## <a name="introduction-to-playbook-role-for-azure"></a>Introducción al rol de cuaderno de estrategias de Azure

El [rol de cuaderno de estrategias azure_preview_module](https://galaxy.ansible.com/Azure/azure_preview_modules/) incluye todos los módulos más recientes de Azure. Las actualizaciones y correcciones de errores se realizan de manera más oportuna que en la versión oficial de Ansible. Si usa Ansible para el aprovisionamiento de recursos de Azure, se recomienda instalar el rol de cuaderno de estrategias `azure_preview_module`.

El rol de cuaderno de estrategias `azure_preview_module` se publica cada tres semanas.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los roles de cuaderno de estrategias, vea [Creating Reusable Playbooks](https://docs.ansible.com/ansible/latest/playbooks_reuse.html) (Creación de cuadernos de estrategias reutilizables). 
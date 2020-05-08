---
title: Compatibilidad con la operación de traslado por tipo de recurso
description: Enumera los tipos de recursos de Azure que se pueden trasladar a un nuevo grupo de recursos o suscripción.
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: a92da04f5b883661d53fd291dc72351a1c4eafb1
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82610843"
---
# <a name="move-operation-support-for-resources"></a>Compatibilidad con la operación de traslado para recursos
En este artículo se indica si un tipo de recurso de Azure es compatible con la operación de traslado. También proporciona información sobre las condiciones especiales que se deben tener en cuenta a la hora de mover un recurso.

Vaya a un espacio de nombres del proveedor de recursos:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppPlatform](#microsoftappplatform)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureData](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.BatchAI](#microsoftbatchai)
> - [Microsoft.Billing](#microsoftbilling)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.BlockchainTokens](#microsoftblockchaintokens)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.Cognition](#microsoftcognition)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Consumption](#microsoftconsumption)
> - [Microsoft.Container](#microsoftcontainer)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerInsights](#microsoftcustomerinsights)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataConnect](#microsoftdataconnect)
> - [Microsoft.DataExchange](#microsoftdataexchange)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLake](#microsoftdatalake)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DataProtection](#microsoftdataprotection)
> - [Microsoft.DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DevOps](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DigitalTwins](#microsoftdigitaltwins)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Experimentation](#microsoftexperimentation)
> - [Microsoft.Falcon](#microsoftfalcon)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.Hydra](#microsofthydra)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kubernetes](#microsoftkubernetes)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft.LocationServices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft.MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Microsoft.MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningOperationalization](#microsoftmachinelearningoperationalization)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.Maintenance](#microsoftmaintenance)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.ManagedNetwork](#microsoftmanagednetwork)
> - [Microsoft.ManagedServices](#microsoftmanagedservices)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.ObjectStore](#microsoftobjectstore)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PortalSdk](#microsoftportalsdk)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft.ProjectOxford](#microsoftprojectoxford)
> - [Microsoft.ProviderHub](#microsoftproviderhub)
> - [Microsoft.Quantum](#microsoftquantum)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.RedHatOpenShift](#microsoftredhatopenshift)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.Services](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.SqlVM](#microsoftsqlvm)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft.Subscription](#microsoftsubscription)
> - [microsoft.support](#microsoftsupport)
> - [Microsoft.Synapse](#microsoftsynapse)
> - [Microsoft.TerraformOSS](#microsoftterraformoss)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Token](#microsofttoken)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.VnfManager](#microsoftvnfmanager)
> - [Microsoft.VSOnline](#microsoftvsonline)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsESU](#microsoftwindowsesu)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | domainservices | No | No |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | tenants | No | No |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | configuraciones | No | No |
> | de películas | No | No |
> | suppressions | No | No |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | actionRules | Sí | Sí |
> | alerts | No | No |
> | alertssummary | No | No |
> | smartdetectoralertrules | Sí | Sí |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | servers | Sí | Sí |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | service | Sí | Sí |

> [!IMPORTANT]
> Un servicio API Management establecido en la SKU de consumo no se puede mover.

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | configurationstores | Sí | Sí |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | spring | Sí | Sí |

## <a name="microsoftappservice"></a>Microsoft.AppService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | apiapps | No | No |
> | appidentities | No | No |
> | gateways | No | No |

> [!IMPORTANT]
> Vea [Guía de movimiento de App Service](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checkaccess | No | No |
> | denyassignments | No | No |
> | findorphanroleassignments | No | No |
> | locks | No | No |
> | permisos | No | No |
> | policyassignments | No | No |
> | policydefinitions | No | No |
> | policysetdefinitions | No | No |
> | roleassignments | No | No |
> | roleassignmentsusagemetrics | No | No |
> | roledefinitions | No | No |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | automationaccounts | Sí | Sí |
> | automationaccounts/configurations | Sí | Sí |
> | automationaccounts/runbooks | Sí | Sí |

> [!IMPORTANT]
> Los runbooks deben encontrarse en el mismo grupo de recursos que la cuenta de Automation.

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | b2cdirectories | Sí | Sí |

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hybriddatamanagers | No | No |
> | postgresinstances | No | No |
> | sqlinstances | No | No |
> | sqlserverregistrations | Sí | Sí |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | registrations | Sí | Sí |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | batchaccounts | Sí | Sí |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | No | No |
> | fileservers | No | No |
> | jobs | No | No |
> | workspaces | No | No |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | billingperiods | No | No |
> | billingpermissions | No | No |
> | billingroleassignments | No | No |
> | billingroledefinitions | No | No |
> | createbillingroleassignment | No | No |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | mapapis | No | No |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | biztalk | No | No |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | blockchainmembers | No | No |
> | cordamembers | No | No |
> | watchers | No | No |

## <a name="microsoftblockchaintokens"></a>Microsoft.BlockchainTokens

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | tokenservices | No | No |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | blueprintassignments | No | No |
> | blueprints | No | No |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | botservices | Sí | Sí |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | redis | Sí | Sí |

> [!IMPORTANT]
> Si la instancia de Azure Redis Cache está configurada con una red virtual, la instancia no se puede mover a otra suscripción. Consulte las [Limitaciones de movimiento de redes](./move-limitations/networking-move-limitations.md).

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | cdnwebapplicationfirewallpolicies | Sí | Sí |
> | profiles | Sí | Sí |
> | profiles/endpoints | Sí | Sí |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | certificateorders | Sí | Sí |

> [!IMPORTANT]
> Vea [Guía de movimiento de App Service](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | domainnames | Sí | No |
> | virtualmachines | Sí | No |

> [!IMPORTANT]
> Vea [Guía de movimiento de implementación clásica](./move-limitations/classic-model-move-limitations.md). Los recursos de implementación clásica se pueden mover entre suscripciones con una operación específica de ese escenario.

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | networksecuritygroups | No | No |
> | reservedips | No | No |
> | virtualnetworks | No | No |

> [!IMPORTANT]
> Vea [Guía de movimiento de implementación clásica](./move-limitations/classic-model-move-limitations.md). Los recursos de implementación clásica se pueden mover entre suscripciones con una operación específica de ese escenario.

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | storageaccounts | Sí | No |

> [!IMPORTANT]
> Vea [Guía de movimiento de implementación clásica](./move-limitations/classic-model-move-limitations.md). Los recursos de implementación clásica se pueden mover entre suscripciones con una operación específica de ese escenario.

## <a name="microsoftcognition"></a>Microsoft.Cognition

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | syntheticsaccounts | No | No |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Sí | Sí |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | availabilitysets | Sí | Sí |
> | diskencryptionsets | No | No |
> | disks | Sí | Sí |
> | galleries | No | No |
> | galleries/images | No | No |
> | galleries/images/versions | No | No |
> | hostgroups | No | No |
> | hostgroups/hosts | No | No |
> | images | Sí | Sí |
> | proximityplacementgroups | Sí | Sí |
> | restorepointcollections | No | No |
> | sharedvmextensions | No | No |
> | sharedvmimages | No | No |
> | sharedvmimages/versions | No | No |
> | snapshots | Sí | Sí |
> | sshpublickeys | No | No |
> | virtualmachines | Sí | Sí |
> | virtualmachines/extensions | Sí | Sí |
> | virtualmachinescalesets | Sí | Sí |

> [!IMPORTANT]
> Vea [Guía de movimiento de Virtual Machines](./move-limitations/virtual-machines-move-limitations.md).

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | aggregatedcost | No | No |
> | balances | No | No |
> | budgets | No | No |
> | charges | No | No |
> | costtags | No | No |
> | credits | No | No |
> | events | No | No |
> | forecasts | No | No |
> | lots | No | No |
> | marketplaces | No | No |
> | operationresults | No | No |
> | operationstatus | No | No |
> | pricesheets | No | No |
> | products | No | No |
> | reservationdetails | No | No |
> | reservationrecommendations | No | No |
> | reservationsummaries | No | No |
> | reservationtransactions | No | No |
> | etiquetas | No | No |
> | tenants | No | No |
> | terms | No | No |
> | usagedetails | No | No |

## <a name="microsoftcontainer"></a>Microsoft.Container

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | containergroups | No | No |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | containergroups | No | No |
> | serviceassociationlinks | No | No |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | registries | Sí | Sí |
> | registries / agentpools | No | No |
> | registries/buildtasks | Sí | Sí |
> | registries/replications | Sí | Sí |
> | registries / taskruns | Sí | Sí |
> | registries/tasks | Sí | Sí |
> | registries/webhooks | Sí | Sí |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | containerservices | No | No |
> | managedclusters | No | No |
> | openshiftmanagedclusters | No | No |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applications | No | No |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | No | No |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | alerts | No | No |
> | budgets | No | No |
> | conectores | Sí | Sí |
> | dimensions | No | No |
> | exports | No | No |
> | externalsubscriptions | No | No |
> | forecast | No | No |
> | Query | No | No |
> | reportconfigs | No | No |
> | reports | No | No |
> | showbackrules | No | No |
> | views | No | No |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hubs | No | No |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | associations | No | No |
> | resourceproviders | Sí | Sí |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | jobs | No | No |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | databoxedgedevices | No | No |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | workspaces | No | No |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | catalogs | Sí | Sí |
> | datacatalogs | No | No |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | connectionmanagers | No | No |

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | packages | No | No |
> | plans | No | No |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | datafactories | Sí | Sí |
> | factories | Sí | Sí |

## <a name="microsoftdatalake"></a>Microsoft.DataLake

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | datalakeaccounts | No | No |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Sí | Sí |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Sí | Sí |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | services | No | No |
> | services/projects | No | No |
> | slots | No | No |

## <a name="microsoftdataprotection"></a>Microsoft.DataProtection

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | backupvaults | No | No |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Sí | Sí |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | servers | Sí | Sí |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | servers | Sí | Sí |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | servergroups | No | No |
> | servers | Sí | Sí |
> | serversv2 | Sí | Sí |
> | singleservers | Sí | Sí |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | artifactsources | Sí | Sí |
> | rollouts | Sí | Sí |
> | servicetopologies | Sí | Sí |
> | servicetopologies/services | Sí | Sí |
> | servicetopologies/services/serviceunits | Sí | Sí |
> | steps | Sí | Sí |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applicationgroups | No | No |
> | hostpools | No | No |
> | workspaces | No | No |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | elasticpools | No | No |
> | elasticpools/iothubtenants | No | No |
> | iothubs | Sí | Sí |
> | provisioningservices | Sí | Sí |

## <a name="microsoftdevops"></a>Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | pipelines | Sí | Sí |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | controllers | Sí | Sí |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | labcenters | No | No |
> | labs | Sí | No |
> | labs/environments | Sí | Sí |
> | labs/servicerunners | Sí | Sí |
> | labs/virtualmachines | Sí | No |
> | schedules | Sí | Sí |

## <a name="microsoftdigitaltwins"></a>Microsoft.DigitalTwins

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | digitaltwinsinstances | No | No |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | databaseaccounts | Sí | Sí |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | domains | Sí | Sí |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | services | Sí | Sí |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | domains | Sí | Sí |
> | eventSubscriptions | No: no se puede mover de forma independiente, sino que se hace automáticamente con el recurso suscrito. | No: no se puede mover de forma independiente, sino que se hace automáticamente con el recurso suscrito. |
> | eventsubscriptions | No: no se puede mover de forma independiente, sino que se hace automáticamente con el recurso suscrito. | No: no se puede mover de forma independiente, sino que se hace automáticamente con el recurso suscrito. |
> | extensiontopics | No | No |
> | partnernamespaces | Sí | Sí |
> | partnerregistrations | No | No |
> | partnertopics | Sí | Sí |
> | systemtopics | Sí | Sí |
> | topics | Sí | Sí |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | Sí | Sí |
> | espacios de nombres | Sí | Sí |

## <a name="microsoftexperimentation"></a>Microsoft.Experimentation

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | experimentworkspaces | No | No |

## <a name="microsoftfalcon"></a>Microsoft.Falcon

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | espacios de nombres | Sí | Sí |

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | No | No |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | automanagedaccounts | No | No |
> | automanagedvmconfigurationprofiles | No | No |
> | guestconfigurationassignments | No | No |
> | software | No | No |
> | softwareupdateprofile | No | No |
> | softwareupdates | No | No |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hanainstances | No | No |
> | sapmonitors | Sí | Sí |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | Sí | Sí |

> [!IMPORTANT]
> Puede mover clústeres de HDInsight a una nueva suscripción o un nuevo grupo de recursos. Sin embargo, no puede mover entre suscripciones los recursos de red vinculados al clúster de HDInsight (por ejemplo, la red virtual, una NIC o un equilibrador de carga). Además, tampoco se puede mover a un nuevo grupo de recursos una NIC que está conectada a una máquina virtual del clúster.
>
> Al mover un clúster de HDInsight a una nueva suscripción, mueva primero otros recursos (por ejemplo, la cuenta de almacenamiento). Después, mover el clúster de HDInsight.

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | services | Sí | Sí |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | machines | Sí | Sí |
> | machines/extensions | No | No |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | datamanagers | Sí | Sí |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | components | No | No |
> | networkscopes | No | No |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | jobs | Sí | Sí |

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | actiongroups | Sí | Sí |
> | activitylogalerts | No | No |
> | alertrules | Sí | Sí |
> | autoscalesettings | Sí | Sí |
> | baseline | No | No |
> | calculatebaseline | No | No |
> | components | Sí | Sí |
> | datacollectionrules | No | No |
> | diagnosticsettings | No | No |
> | diagnosticsettingscategories | No | No |
> | eventtypes | No | No |
> | extendeddiagnosticsettings | No | No |
> | guestdiagnosticsettings | No | No |
> | logdefinitions | No | No |
> | logs | No | No |
> | metricalerts | No | No |
> | metricbaselines | No | No |
> | metricdefinitions | No | No |
> | metricnamespaces | No | No |
> | Métricas | No | No |
> | myworkbooks | No | No |
> | notificationgroups | No | No |
> | privatelinkscopes | Sí | Sí |
> | scheduledqueryrules | Sí | Sí |
> | Topología | No | No |
> | transacciones | No | No |
> | vminsightsonboardingstatuses | No | No |
> | webtests | Sí | Sí |
> | workbooks | Sí | Sí |
> | workbooktemplates | Sí | Sí |

> [!IMPORTANT]
> Asegúrese de que el movimiento a una nueva suscripción no exceda las [cuotas de suscripción](azure-subscription-service-limits.md#azure-monitor-limits).

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | iotapps | Sí | Sí |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Sí | Sí |
> | graph | Sí | Sí |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hsmpools | No | No |
> | vaults | Sí | Sí |

> [!IMPORTANT]
> Los almacenes de claves usados para el cifrado de discos no se pueden mover a grupos de recursos de la misma suscripción ni entre suscripciones.

## <a name="microsoftkubernetes"></a>Microsoft.Kubernetes

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | connectedclusters | No | No |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | Sí | Sí |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | labaccounts | No | No |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | No | No |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | No | No |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hostingenvironments | No | No |
> | integrationaccounts | Sí | Sí |
> | integrationserviceenvironments | Sí | No |
> | integrationserviceenvironments / managedapis | Sí | No |
> | isolatedenvironments | No | No |
> | workflows | Sí | Sí |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | commitmentplans | No | No |
> | webservices | Sí | No |
> | workspaces | Sí | Sí |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | operationalizationclusters | No | No |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | No | No |
> | accounts/workspaces | No | No |
> | accounts/workspaces/projects | No | No |
> | teamaccounts | No | No |
> | teamaccounts/workspaces | No | No |
> | teamaccounts/workspaces/projects | No | No |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | No | No |

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft.MachineLearningOperationalization

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hostingaccounts | No | No |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | workspaces | No | No |
> | workspaces/computes | No | No |

## <a name="microsoftmaintenance"></a>Microsoft.Maintenance

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applyupdates | No | No |
> | configurationassignments | No | No |
> | maintenanceconfigurations | Sí | Sí |
> | updates | No | No |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Identidades | No | No |
> | userassignedidentities | No | No |

## <a name="microsoftmanagednetwork"></a>Microsoft.ManagedNetwork

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | managednetworks | No | No |
> | managednetworks / managednetworkgroups | No | No |
> | managednetworks / managednetworkpeeringpolicies | No | No |
> | notificación | No | No |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | registrationassignments | No | No |
> | registrationdefinitions | No | No |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Sí | Sí |
> | accounts / privateatlases | Sí | Sí |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | classicdevservices | No | No |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | mediaservices | Sí | Sí |
> | mediaservices/liveevents | Sí | Sí |
> | mediaservices/streamingendpoints | Sí | Sí |

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Spring

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | appclusters | No | No |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | assessmentprojects | Sí | Sí |
> | migrateprojects | Sí | Sí |
> | movecollections | No | No |
> | projects | No | No |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | netappaccounts | No | No |
> | netappaccounts / backuppolicies | No | No |
> | netappaccounts/capacitypools | No | No |
> | netappaccounts/capacitypools/volumes | No | No |
> | netappaccounts/capacitypools/volumes/mounttargets | No | No |
> | netappaccounts/capacitypools/volumes/snapshots | No | No |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applicationgateways | No | No |
> | applicationgatewaywebapplicationfirewallpolicies | No | No |
> | applicationsecuritygroups | Sí | Sí |
> | azurefirewalls | Sí | Sí |
> | bastionhosts | No | No |
> | connections | Sí | Sí |
> | ddoscustompolicies | Sí | Sí |
> | ddosprotectionplans | No | No |
> | dnszones | Sí | Sí |
> | expressroutecircuits | No | No |
> | expressroutegateways | No | No |
> | firewallpolicies | Sí | Sí |
> | frontdoors | No | No |
> | frontdoorwebapplicationfirewallpolicies | No | No |
> | ipgroups | Sí | Sí |
> | loadbalancers | Sí: SKU básico<br>No: SKU estándar | Sí: SKU básico<br>No: SKU estándar |
> | localnetworkgateways | Sí | Sí |
> | natgateways | Sí | Sí |
> | networkexperimentprofiles | Sí | Sí |
> | networkintentpolicies | Sí | Sí |
> | networkinterfaces | Sí | Sí |
> | networkprofiles | No | No |
> | networksecuritygroups | Sí | Sí |
> | networkwatchers | Sí | No |
> | networkwatchers/connectionmonitors | Sí | No |
> | networkwatchers / flowlogs | Sí | No |
> | networkwatchers/pingmeshes | Sí | No |
> | p2svpngateways | No | No |
> | privatednszones | Sí | Sí |
> | privatednszones/virtualnetworklinks | Sí | Sí |
> | privateendpointredirectmaps | No | No |
> | privateendpoints | Sí | Sí |
> | privatelinkservices | No | No |
> | publicipaddresses | Sí: SKU básico<br>No: SKU estándar | Sí: SKU básico<br>No: SKU estándar |
> | publicipprefixes | Sí | Sí |
> | routefilters | No | No |
> | routetables | Sí | Sí |
> | serviceendpointpolicies | Sí | Sí |
> | trafficmanagerprofiles | Sí | Sí |
> | virtualhubs | No | No |
> | virtualnetworkgateways | Sí | Sí |
> | virtualnetworks | Sí | Sí |
> | virtualnetworktaps | No | No |
> | virtualrouters | Sí | Sí |
> | virtualwans | No | No |
> | vpngateways (Virtual WAN) | No | No |
> | vpnserverconfigurations | No | No |
> | vpnsites (Virtual WAN) | No | No |
> | webapplicationfirewallpolicies | Sí | Sí |

> [!IMPORTANT]
> Consulte [Guía de movimiento de redes](./move-limitations/networking-move-limitations.md).

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | espacios de nombres | Sí | Sí |
> | namespaces/notificationhubs | Sí | Sí |

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | osnamespaces | Sí | Sí |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | No | No |
> | storageinsightconfigs | No | No |
> | workspaces | Sí | Sí |

> [!IMPORTANT]
> Asegúrese de que el movimiento a una nueva suscripción no exceda las [cuotas de suscripción](azure-subscription-service-limits.md#azure-monitor-limits).

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | managementassociations | No | No |
> | managementconfigurations | Sí | Sí |
> | solutions | Sí | Sí |
> | views | Sí | Sí |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | peerings | Sí | Sí |
> | peeringservices | No | No |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | policyevents | No | No |
> | policystates | No | No |
> | policytrackedresources | No | No |
> | remediations | No | No |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | dashboards | Sí | Sí |

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | rootresources | No | No |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | workspacecollections | Sí | Sí |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | capacities | Sí | Sí |

## <a name="microsoftprojectbabylon"></a>Microsoft.ProjectBabylon

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | No | No |

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | No | No |

## <a name="microsoftproviderhub"></a>Microsoft.ProviderHub

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | rollouts | No | No |

## <a name="microsoftquantum"></a>Microsoft.Quantum

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | workspaces | No | No |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | backupprotecteditems | No | No |
> | replicationeligibilityresults | No | No |
> | vaults | Sí | Sí |

> [!IMPORTANT]
> Vea [Guía de movimiento de Recovery Services](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftredhatopenshift"></a>Microsoft.RedHatOpenShift

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | openshiftclusters | No | No |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | espacios de nombres | Sí | Sí |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Consultas | Sí | Sí |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | availabilitystatuses | No | No |
> | childavailabilitystatuses | No | No |
> | childresources | No | No |
> | events | No | No |
> | Notificaciones | No | No |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | deploymentscripts | No | No |
> | vínculos | No | No |
> | etiquetas | No | No |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applications | Sí | No |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | searchservices | Sí | Sí |

> [!IMPORTANT]
> No puede mover varios recursos de Search de regiones diferentes en una operación. En su lugar, muévalos en operaciones independientes.

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | adaptivenetworkhardenings | No | No |
> | advancedthreatprotectionsettings | No | No |
> | assessmentmetadata | No | No |
> | assessments | No | No |
> | automations | Sí | Sí |
> | complianceresults | No | No |
> | compliances | No | No |
> | datacollectionagents | No | No |
> | devicesecuritygroups | No | No |
> | informationprotectionpolicies | No | No |
> | iotsecuritysolutions | Sí | Sí |
> | servervulnerabilityassessments | No | No |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | aggregations | No | No |
> | alertrules | No | No |
> | alertruletemplates | No | No |
> | bookmarks | No | No |
> | cases | No | No |
> | dataconnectors | No | No |
> | dataconnectorscheckrequirements | No | No |
> | entities | No | No |
> | entityqueries | No | No |
> | incidents | No | No |
> | officeconsents | No | No |
> | configuración | No | No |

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | gateways | No | No |
> | nodes | No | No |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | espacios de nombres | Sí | Sí |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applications | No | No |
> | clusters | Sí | Sí |
> | clusters/applications | No | No |
> | containergroups | No | No |
> | containergroupsets | No | No |
> | edgeclusters | No | No |
> | managedclusters | No | No |
> | networks | No | No |
> | secretstores | No | No |
> | volumes | No | No |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applications | Sí | Sí |
> | containergroups | No | No |
> | gateways | Sí | Sí |
> | networks | Sí | Sí |
> | secrets | Sí | Sí |
> | volumes | Sí | Sí |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | rollouts | No | No |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | signalr | Sí | Sí |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hybridusebenefits | No | No |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applicationdefinitions | No | No |
> | applications | No | No |
> | jitrequests | No | No |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | instancepools | No | No |
> | locations | Sí | Sí |
> | managedinstances | No | No |
> | managedinstances/databases | No | No |
> | servers | Sí | Sí |
> | servers/databases | Sí | Sí |
> | servers/elasticpools | Sí | Sí |
> | servers / jobaccounts | Sí | Sí |
> | servers / jobagents | Sí | Sí |
> | virtualclusters | Sí | Sí |

> [!IMPORTANT]
> La base de datos y el servidor deben residir en el mismo grupo de recursos. Cuando se mueve un servidor SQL Server, se mueven también todas sus bases de datos. Este comportamiento se aplica a las bases de datos de Azure SQL Database y Azure SQL Data Warehouse.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | sqlvirtualmachinegroups | Sí | Sí |
> | sqlvirtualmachines | Sí | Sí |

## <a name="microsoftsqlvm"></a>Microsoft.SqlVM

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | dwvm | No | No |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | storageaccounts | Sí | Sí |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | caches | No | No |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | Sí | Sí |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | No | No |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | No | No |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | managers | No | No |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | streamingjobs | Sí | Sí |

> [!IMPORTANT]
> Los trabajos de Stream Analytics no se pueden mover si se encuentran en estado de ejecución.

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | environments | No | No |
> | environments/eventsources | No | No |
> | instances | No | No |
> | instances/environments | No | No |
> | instances/environments/eventsources | No | No |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | createsubscription | No | No |

## <a name="microsoftsupport"></a>microsoft.support

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | supporttickets | No | No |

## <a name="microsoftsynapse"></a>Microsoft.Synapse

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | workspaces | No | No |
> | workspaces / bigdatapools | No | No |
> | workspaces / sqlpools | No | No |

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | providerregistrations | No | No |
> | resources | No | No |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | environments | Sí | Sí |
> | environments/eventsources | Sí | Sí |
> | environments/referencedatasets | Sí | Sí |

## <a name="microsofttoken"></a>Microsoft.Token

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | almacenes | Sí | Sí |

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | imagetemplates | No | No |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | account | No | No |
> | account/extension | Sí | Sí |
> | account/project | Sí | Sí |

> [!IMPORTANT]
> Para cambiar la suscripción de Azure DevOps, vea [Cambiar la suscripción de Azure usada para la facturación](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | dedicatedcloudnodes | No | No |
> | dedicatedcloudservices | No | No |
> | virtualmachines | No | No |

## <a name="microsoftvnfmanager"></a>Microsoft.VnfManager

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | devices | No | No |
> | vnfs | No | No |

## <a name="microsoftvsonline"></a>Microsoft.VSOnline

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Sí | Sí |
> | plans | Sí | Sí |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | certificates | No | Sí |
> | connectiongateways | Sí | Sí |
> | connections | Sí | Sí |
> | customapis | Sí | Sí |
> | hostingenvironments | No | No |
> | kubeenvironments | Sí | Sí |
> | serverfarms | Sí | Sí |
> | sites | Sí | Sí |
> | sites/premieraddons | Sí | Sí |
> | sites/slots | Sí | Sí |
> | staticsites | No | No |

> [!IMPORTANT]
> Vea [Guía de movimiento de App Service](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftwindowsesu"></a>Microsoft.WindowsESU

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | multipleactivationkeys | No | No |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | deviceservices | No | No |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | components | No | No |
> | monitorinstances | No | No |
> | monitors | No | No |
> | notificationsettings | No | No |

## <a name="third-party-services"></a>Servicios de terceros

Actualmente los servicios de terceros no son compatibles con la operación de traslado.

## <a name="next-steps"></a>Pasos siguientes
Para ver los comandos para trasladar recursos, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](move-resource-group-and-subscription.md).

Para obtener los mismos datos como un archivo de valores separados por comas, descargue [move-support-resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv).

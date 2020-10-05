---
title: Cifrado de Azure HDInsight en tránsito
description: Obtenga información sobre las características de seguridad para proporcionar cifrado en tránsito para el clúster de Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/24/2020
ms.openlocfilehash: 25e38beb561ee954db2987643775f3a3c6e05737
ms.sourcegitcommit: 5d7f8c57eaae91f7d9cf1f4da059006521ed4f9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89668777"
---
# <a name="ipsec-encryption-in-transit-for-azure-hdinsight"></a>Cifrado IPSec en tránsito para Azure HDInsight

En este artículo se describe la implementación del cifrado en tránsito para la comunicación entre los nodos de clúster de Azure HDInsight.

> [!Note]
> Actualmente, el cifrado en tránsito está habilitado para las siguientes regiones: Este de EE. UU., Centro-sur de EE. UU. y Oeste de EE. UU 2. 

## <a name="background"></a>Información previa

Azure HDInsight ofrece una variedad de características de seguridad para proteger los datos empresariales. Estas soluciones se agrupan bajo los pilares de la seguridad perimetral, la autenticación, la autorización, la auditoría, el cifrado y el cumplimiento. El cifrado se puede aplicar a los datos en reposo y en tránsito.

El cifrado en reposo se realiza mediante el cifrado del lado servidor en las cuentas de almacenamiento de Azure, así como el cifrado de disco en las VM de Azure que forman parte del clúster de HDInsight.

El cifrado de datos en tránsito en HDInsight se consigue con la [Seguridad de la capa de transporte (TLS)](https://docs.microsoft.com/azure/hdinsight/transport-layer-security) para acceder a las puertas de enlace de clúster y con el [Protocolo de seguridad de Internet (IPSec)](https://en.wikipedia.org/wiki/IPsec) entre nodos del clúster. IPSec se puede habilitar opcionalmente entre todos los nodos principales, de trabajo, perimetrales y de Zookeeper. No está habilitado para el tráfico entre nodos de puerta de enlace o de [agente de identidad](https://docs.microsoft.com/azure/hdinsight/domain-joined/identity-broker), que son máquinas virtuales basadas en Windows y otros nodos basados en Linux en el clúster.

## <a name="enable-encryption-in-transit"></a>Habilitar el cifrado en tránsito

### <a name="azure-portal"></a>Azure portal

Para crear un nuevo clúster con el cifrado en tránsito habilitado mediante Azure Portal, siga estos pasos:

1. Inicie el proceso de creación de clústeres normal. Consulte los pasos iniciales de creación de clústeres en [Creación de clústeres basados en Linux en HDInsight desde Azure Portal](../hdinsight-hadoop-create-linux-clusters-portal.md).
1. Complete las pestañas **Aspectos básicos** y **Almacenamiento**. Continúe a la pestaña **Seguridad y redes**.

    :::image type="content" source="media/encryption-in-transit/create-cluster-security-networking-tab.png" alt-text="Creación del clúster: pestaña Seguridad y redes.":::

1. En la pestaña **Seguridad y redes**, haga clic en la casilla **Habilitar el cifrado en tránsito**.

    :::image type="content" source="media/encryption-in-transit/enable-encryption-in-transit.png" alt-text="Creación del clúster: pestaña Seguridad y redes.":::

### <a name="create-a-cluster-with-encryption-in-transit-enabled-through-the-azure-cli"></a>Cree un clúster con el cifrado en tránsito habilitado mediante la CLI de Azure

El cifrado en tránsito se habilita mediante la propiedad `isEncryptionInTransitEnabled`.

Puede [descargar una plantilla de ejemplo y un archivo de parámetros](https://github.com/Azure-Samples/hdinsight-enterprise-security). Antes de usar la plantilla y el fragmento de código de la CLI de Azure que aparecen a continuación, reemplace estos marcadores de posición por sus valores correctos:

| Marcador de posición | Descripción |
|---|---|
| `<SUBSCRIPTION_ID>` | Identificador de la suscripción de Azure |
| `<RESOURCE_GROUP>` | El grupo de recursos donde quiere crear el nuevo clúster y la cuenta de almacenamiento. |
| `<STORAGEACCOUNTNAME>` | La cuenta de almacenamiento existente que se debe usar con el clúster. El nombre debe tener el formato `ACCOUNTNAME.blob.core.windows.net`. |
| `<CLUSTERNAME>` | El nombre del clúster de HDInsight. |
| `<PASSWORD>` | La contraseña elegida para iniciar sesión en el clúster mediante SSH y el panel de Ambari. |
| `<VNET_NAME>` | Red virtual en que se implementará el clúster. |

El siguiente fragmento de código realiza los pasos iniciales siguientes:

1. Inicia sesión en la cuenta de Azure.
1. Establece la suscripción activa donde se realizarán las operaciones de creación.
1. Crea un nuevo grupo de recursos para las nuevas actividades de implementación.
1. Implementa la plantilla para crear un nuevo clúster.

```azurecli
az login
az account set --subscription <SUBSCRIPTION_ID>

# Create resource group
az group create --name <RESOURCEGROUPNAME> --location eastus2

az group deployment create --name HDInsightEnterpriseSecDeployment \
    --resource-group <RESOURCEGROUPNAME> \
    --template-file hdinsight-enterprise-security.json \
    --parameters parameters.json
```

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a la seguridad de la empresa en Azure HDInsight](hdinsight-security-overview.md)
* [Sincronización de los usuarios de Azure Active Directory en un clúster de HDInsight](../disk-encryption.md).

---
title: 'Tutorial: Requisitos previos para Cognitive Services en Azure Synapse'
description: Tutorial sobre cómo configurar los requisitos previos para usar Cognitive Services en Azure Synapse
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: cf76a3d09da83693791873feb9e6ab03c432e47f
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96464402"
---
# <a name="tutorial-pre-requisites-for-using-cognitive-services-in-azure-synapse"></a>Tutorial: Requisitos previos para el uso de Cognitive Services en Azure Synapse

En este tutorial, obtendrá información sobre cómo configurar los requisitos previos para aprovechar de forma segura Cognitive Services en Azure Synapse.

Esta tutorial abarca lo siguiente:
> [!div class="checklist"]
> - Creación de un recurso de Cognitive Services. Por ejemplo, Text Analytics o Anomaly Detector.
> - Almacenamiento de la clave de autenticación para los recursos de Cognitive Services como secretos en Azure Key Vault y configuración del acceso para el área de trabajo de Azure Synapse.
> - Creación de un servicio vinculado de Azure Key Vault en el área de trabajo de Azure Synapse Analytics.

Si no tiene una suscripción a Azure, [cree una cuenta gratuita antes de empezar](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Requisitos previos

- Necesitará un [área de trabajo de Synapse Analytics](../get-started-create-workspace.md) con una cuenta de almacenamiento de ADLS Gen2 configurada como almacenamiento predeterminado. Debe ser el **Colaborador de datos de blobs de almacenamiento** del sistema de archivos de ADLS Gen2 con el que quiere trabajar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Portal de Azure](https://portal.azure.com/)

## <a name="create-a-cognitive-services-resource"></a>Creación de un recurso de Cognitive Services

[Azure Cognitive Services](https://go.microsoft.com/fwlink/?linkid=2147492) incluye muchos tipos diferentes de servicios. A continuación se muestran algunos ejemplos que se usan en los tutoriales de Synapse.

### <a name="create-an-anomaly-detector-resource"></a>Creación de un recurso de Anomaly Detector
Cree un recurso de [Anomaly Detector](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) en Azure Portal.

![Creación de un recurso de Anomaly Detector](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00a.png)

### <a name="create-a-text-analytics-resource"></a>Creación de un recurso de Text Analytics
Cree un recurso de [Text Analytics](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) en Azure Portal.

![Creación de un recurso de Text Analytics](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00b.png)

## <a name="create-key-vault-and-configure-secrets-and-access"></a>Creación de un recurso de Key Vault y configuración de los secretos y el acceso

1. Cree un recurso de [Key Vault](https://ms.portal.azure.com/#create/Microsoft.KeyVault) en Azure Portal.
2. Vaya a **Key Vault -> Directivas de acceso** y conceda permisos a la [identidad administrada del área de trabajo de Azure Synapse](https://docs.microsoft.com/azure/synapse-analytics/security/synapse-workspace-managed-identity) para leer secretos de Azure Key Vault.

>Asegúrese de que se guardan los cambios en la directiva. Este paso es fácil de pasar por alto.

![Agregar directiva de acceso](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00c.png)

3. Vaya al recurso de Cognitive Services, por ejemplo, **Anomaly Detector -> Claves y punto de conexión** y copie cualquiera de las dos claves en el portapapeles.

4. Vaya a **Key Vault -> Secreto** para crear un secreto nuevo. Especifique el nombre del secreto y, a continuación, pegue la clave del paso anterior en el campo "Valor". Por último, haga clic en **Crear**.

![Creación de un secreto](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00d.png)

> Asegúrese de recordar o anotar este nombre de secreto. La usará más adelante cuando se conecte a Cognitive Services desde Azure Synapse Studio.

## <a name="create-azure-keyvault-linked-service-in-azure-synapse"></a>Creación de un servicio vinculado de Azure Key Vault en Azure Synapse

1. Abra el área de trabajo en Azure Synapse Studio. Vaya a **Administrar -> Servicios vinculados**. Cree el servicio vinculado "Azure Key Vault" que apunta al almacén de claves que acabamos de crear. A continuación, compruebe la conexión; para ello, haga clic en el botón "Probar conexión" y compruebe si está en color verde. Si todo funciona bien, haga clic en "Crear" primero y después en "Publicar todo" para guardar el cambio.
![Servicio vinculado](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00e.png)

Ahora está listo para continuar con uno de los tutoriales para usar la experiencia de Azure Cognitive Services en Azure Synapse Studio.

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Análisis de sentimiento con Azure Cognitive Services](tutorial-cognitive-services-sentiment.md)
- [Tutorial: Detección de anomalías con Azure Cognitive Services](tutorial-cognitive-services-sentiment.md)
- [Tutorial: Asistente para puntuación del modelo de Machine Learning para grupos de SQL dedicados](tutorial-sql-pool-model-scoring-wizard.md).
- [Funcionalidades de aprendizaje automático en Azure Synapse Analytics](what-is-machine-learning.md)
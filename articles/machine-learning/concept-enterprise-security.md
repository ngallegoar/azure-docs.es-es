---
title: Seguridad y gobernanza para empresas
titleSuffix: Azure Machine Learning
description: 'Use Azure Machine Learning de forma segura: autenticación, autorización, seguridad de red, cifrado de datos y supervisión.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 09/09/2020
ms.openlocfilehash: fb1f1d098970927ba04c840e77ec0a0b8d76ca02
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94561325"
---
# <a name="enterprise-security-and-governance-for-azure-machine-learning"></a>Seguridad de empresa y gobernanza para Azure Machine Learning

En este artículo conocerá las características de seguridad disponibles con Azure Machine Learning.

Cuando se usa un servicio en la nube, se recomienda restringir el acceso solo a los usuarios que lo necesiten. Para empezar, es necesario comprender el modelo de autenticación y autorización que usa el servicio. Es posible que también quiera restringir el acceso a la red o unir de forma segura los recursos de la red local con la nube. También es fundamental el cifrado de datos, tanto si están en reposo como si se mueven entre los servicios. También puede que quiera crear directivas para aplicar determinadas configuraciones o registrar cuándo se crean configuraciones no compatibles. Por último, debe ser capaz de supervisar el servicio y generar un registro de auditoría de todas las actividades.

> [!NOTE]
> La información de este artículo se aplica a la versión 1.0.83.1 o a versiones posteriores del SDK de Python de Azure Machine Learning.

## <a name="authentication--authorization"></a>Autenticación y autorización

La mayor parte de la autenticación en los recursos de Azure Machine Learning usa Azure Active Directory (Azure AD) para la autenticación y el control de acceso basado en roles (Azure RBAC) para la autorización. Las excepciones a esto son:

* __SSH__: puede habilitar el acceso SSH a algunos recursos de proceso, como las instancias de proceso de Azure Machine Learning. El acceso SSH usa la autenticación basada en claves. Para más información sobre cómo crear claves SSH, consulte el artículo sobre [Creación y administración de claves SSH](../virtual-machines/linux/create-ssh-keys-detailed.md). Para obtener información sobre cómo habilitar el acceso SSH, consulte [Creación y administración de una instancia de proceso de Azure Machine Learning](how-to-create-manage-compute-instance.md).
* __Modelos implementados como servicios web__: las implementaciones de servicios web pueden usar el control de acceso basado en __claves__ o __tokens__. Las claves son cadenas estáticas. Los tokens se recuperan mediante una cuenta de Azure AD. Para obtener más información, consulte [Configuración de la autenticación para los modelos implementados como servicios web](how-to-authenticate-web-service.md).

Los servicios específicos en los que se basa Azure Machine Learning, como los servicios de almacenamiento de datos de Azure, tienen sus propios métodos de autenticación y autorización. Para obtener más información sobre la autenticación de los servicios de almacenamiento, consulte [Conexión con los servicios de Azure Storage](how-to-access-data.md).

### <a name="azure-ad-authentication"></a>Autenticación de Azure AD

Si Azure Active Directory (Azure AD) se ha configurado para usar la autenticación multifactor, también se admite aquí. Este es el proceso de autenticación:

1. El cliente inicia sesión en Azure AD y recibe un token de Azure Resource Manager.  Los usuarios y las entidades de servicio son totalmente compatibles.
1. El cliente presenta el token a Azure Resource Manager y a todas las instancias de Azure Machine Learning.
1. Machine Learning Service proporciona un token al destino de proceso de usuario (por ejemplo, Proceso de Machine Learning). El destino de proceso de usuario usa este token para volver a llamar a Machine Learning Service una vez completada la ejecución. El ámbito se limita al área de trabajo.

[![Autenticación en Azure Machine Learning](media/concept-enterprise-security/authentication.png)](media/concept-enterprise-security/authentication.png#lightbox)

Para obtener más información, consulte [Autenticación de un área de trabajo de Azure Machine Learning](how-to-setup-authentication.md).

### <a name="azure-rbac"></a>Azure RBAC

Puede crear varias áreas de trabajo, y cada área de trabajo se puede compartir entre varias personas. Puede controlar a qué características u operaciones del área de trabajo pueden acceder los usuarios al asignar sus cuentas de Azure AD a los roles de Azure RBAC. Los siguientes son los roles integrados:

* Propietario
* Colaborador
* Lector

Los propietarios y colaboradores pueden usar todos los destinos de proceso y almacenes de datos conectados al área de trabajo.  

En la tabla siguiente se muestran algunas de las principales operaciones de Azure Machine Learning y los roles que pueden realizarlas:

| Operación de Azure Machine Learning | Propietario | Colaborador | Lector |
| ---- |:----:|:----:|:----:|
| Creación del espacio de trabajo | ✓ | ✓ | |
| Compartir área de trabajo | ✓ | |  |
| Crear el destino de proceso | ✓ | ✓ | |
| Asociar el destino de proceso | ✓ | ✓ | |
| Asociar almacenes de datos | ✓ | ✓ | |
| Ejecutar experimento | ✓ | ✓ | |
| Ver ejecuciones/métricas | ✓ | ✓ | ✓ |
| Registro del modelo | ✓ | ✓ | |
| Crear una imagen | ✓ | ✓ | |
| Implementar el servicio web | ✓ | ✓ | |
| Ver modelos/imágenes | ✓ | ✓ | ✓ |
| Llamar a un servicio web | ✓ | ✓ | ✓ |

Si los roles integrados no satisfacen sus necesidades, puede crear roles personalizados. Los roles personalizados controlan todas las operaciones dentro de un área de trabajo, como la creación de un proceso, el envío de una ejecución, el registro de un almacén de datos o la implementación de un modelo. Los roles personalizados pueden tener permisos de lectura, escritura o eliminación en los distintos recursos de un área de trabajo, como clústeres, almacenes de datos, modelos y puntos de conexión. Puede hacer que el rol esté disponible en un nivel de área de trabajo específico, un nivel de grupo de recursos específico o un nivel de suscripción específico. Para más información, consulte [Administración de usuarios y roles en un área de trabajo de Azure Machine Learning](how-to-assign-roles.md).

> [!IMPORTANT]
> Azure Machine Learning depende de otros servicios de Azure, como Azure Blob Storage y Azure Kubernetes Services. Cada servicio de Azure tiene sus propias configuraciones de Azure RBAC. Para lograr el nivel de control de acceso deseado, se recomienda que aplique tanto las configuraciones de Azure RBAC para Azure Machine Learning y como las de los servicios que se usan con Azure Machine Learning.

> [!WARNING]
> Azure Machine Learning es compatible con la colaboración de negocio a negocio de Azure Active Directory, pero no es compatible actualmente con la colaboración de negocio a consumidor de Azure Active Directory.

### <a name="managed-identities"></a>Identidades administradas

Cada área de trabajo también tiene una [identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) asignada por el sistema asociada con el mismo nombre que el área de trabajo. La identidad administrada se usa para acceder de manera segura a los recursos que usa el área de trabajo. Tiene los siguientes permisos sobre los recursos conectados:

| Resource | Permisos |
| ----- | ----- |
| Área de trabajo | Colaborador |
| Cuenta de almacenamiento | Colaborador de datos de blobs de almacenamiento |
| Almacén de claves | Acceso a todas las claves, secretos, certificados |
| Azure Container Registry | Colaborador |
| El grupo de recursos que contiene el área de trabajo | Colaborador |
| El grupo de recursos que contiene el almacén de claves (si es diferente al que contiene el área de trabajo) | Colaborador |

No se recomienda que los administradores revoquen el acceso de la identidad administrada a los recursos mencionados en la tabla anterior. Puede restaurar el acceso mediante la operación de resincronización de claves.

Azure Machine Learning crea una aplicación adicional (el nombre empieza por `aml-` o `Microsoft-AzureML-Support-App-`) con acceso de nivel de colaborador en la suscripción para cada región del área de trabajo. Por ejemplo, si tiene un área de trabajo en la región Este de EE. UU. y otra en la región Norte de Europa en la misma suscripción, verá dos de estas aplicaciones. Estas aplicaciones permiten que Azure Machine Learning le ayude a administrar los recursos de proceso.

También puede configurar sus propias identidades administradas para usarlas con Azure Virtual Machines y el clúster de proceso de Azure Machine Learning. Con una máquina virtual, la identidad administrada se puede usar para acceder al área de trabajo desde el SDK, en lugar de la cuenta de Azure AD del usuario individual. Con un clúster de proceso, la identidad administrada se usa para acceder a recursos como los almacenes de datos seguros a los que el usuario que ejecuta el trabajo de entrenamiento podría no tener acceso. Para obtener más información, consulte [Autenticación de un área de trabajo de Azure Machine Learning](how-to-setup-authentication.md).

## <a name="network-security-and-isolation"></a>Aislamiento y seguridad de la red

Puede usar Azure Virtual Network (VNet) para restringir el acceso físico a recursos de Azure Machine Learning. Las redes virtuales le permiten crear entornos de red que están aislados de manera parcial o total del Internet público. Esto reduce la superficie expuesta a ataques de la solución, así como las posibilidades de que se produzca una filtración de datos.

Para obtener más información, vea los documentos siguientes:

* [Información general sobre la privacidad y el aislamiento de la red virtual](how-to-network-security-overview.md)
* [Protección de los recursos de un área de trabajo](how-to-secure-workspace-vnet.md)
* [Protección de un entorno de entrenamiento](how-to-secure-training-vnet.md)
* [Protección del entorno de inferencia](how-to-secure-inferencing-vnet.md)
* [Uso de Studio en una red virtual protegida](how-to-enable-studio-virtual-network.md)

<a id="encryption-at-rest"></a><a id="azure-blob-storage"></a>

## <a name="data-encryption"></a>Cifrado de datos

Azure Machine Learning usa varios recursos de proceso y almacenes de datos. Para obtener más información sobre cómo cada uno de ellos admite el cifrado de datos en reposo y en tránsito, consulte [Cifrado de datos con Azure Machine Learning](concept-data-encryption.md).

### <a name="microsoft-generated-data"></a>Datos generados por Microsoft

Cuando usa servicios como el aprendizaje automático automatizado, Microsoft puede generar datos transitorios previamente procesados para entrenar varios modelos. Estos datos se guardan en un almacén de datos en el área de trabajo, lo que permite aplicar los controles de acceso y el cifrado de forma adecuada.

También puede que quiera cifrar la [información de diagnóstico registrada desde el punto de conexión implementado](how-to-enable-app-insights.md) en su instancia de Azure Application Insights.

## <a name="monitoring"></a>Supervisión

Hay varios escenarios de supervisión con Azure Machine Learning, en función del rol y de lo que se esté supervisando.

| Role | Supervisión que se debe usar | Descripción |
| ---- | ----- | ----- |
| Administrador, DevOps, MLOps | [Métricas de Azure Monitor](#azure-monitor), [registro de actividades](#activity-log), [examen de vulnerabilidades](#vulnerability-scanning) | Información de nivel de servicio |
| Científico de datos, MLOps | [Supervisión de ejecuciones](#monitor-runs) | Información registrada durante las ejecuciones de entrenamiento |
| MLOps | [Recopilación con datos de modelo](how-to-enable-data-collection.md), [Supervisión con Application Insights](how-to-enable-app-insights.md) | Información registrada por los modelos implementados como servicios web o módulos de IoT Edge|

### <a name="monitor-runs"></a>Supervisión de ejecuciones

Puede supervisar las ejecuciones de los experimentos en Azure Machine Learning, incluida la información de registro de los scripts de entrenamiento. Esta información se puede visualizar a través del SDK, la CLI de Azure y el estudio. Para más información, consulte los siguientes artículos.

* [Inicio, supervisión y cancelación de las ejecuciones de entrenamiento](how-to-manage-runs.md)
* [Habilitamiento de registros](how-to-track-experiments.md)
* [Visualización de registros](how-to-monitor-view-training-logs.md)
* [Visualización de ejecuciones con TensorBoard](how-to-monitor-tensorboard.md)

### <a name="azure-monitor"></a>Azure Monitor

Puede usar las métricas de Azure Monitor para ver y supervisar las métricas del área de trabajo de Azure Machine Learning. En [Azure Portal](https://portal.azure.com), seleccione el área de trabajo y, a continuación, **Métricas**:

[![Captura de pantalla que muestra las métricas de ejemplo de un área de trabajo](media/concept-enterprise-security/workspace-metrics.png)](media/concept-enterprise-security/workspace-metrics-expanded.png#lightbox)

Las métricas incluye información sobre ejecuciones, implementaciones y registros.

Para más información, consulte [Métricas en Azure Monitor](../azure-monitor/platform/data-platform-metrics.md).

### <a name="activity-log"></a>Registro de actividades

Puede examinar el registro de actividad de un área de trabajo para ver diversas operaciones que se realizan en el área de trabajo. El registro incluye información básica como el nombre de la operación, el iniciador del evento y la marca de tiempo.

En esta captura de pantalla se muestra el registro de actividad de un área de trabajo:

[![Captura de pantalla que muestra el registro de actividad de un área de trabajo](media/concept-enterprise-security/workspace-activity-log.png)](media/concept-enterprise-security/workspace-activity-log-expanded.png#lightbox)

Los detalles de la solicitud de puntuación se almacenan en Application Insights. Application Insights se crea en la suscripción cuando se crea un área de trabajo. La información registrada incluye campos como estos:

* HTTPMethod
* UserAgent
* ComputeType
* RequestUrl
* StatusCode
* RequestId
* Duration

> [!IMPORTANT]
> Algunas acciones del área de trabajo de Azure Machine Learning no registran información en el registro de actividad. Por ejemplo, el inicio de una ejecución de entrenamiento y el registro de un modelo no se registran.
>
> Algunas de estas acciones aparecen en el área **Actividades** del área de trabajo, pero estas notificaciones no indican quién inició la actividad.

### <a name="vulnerability-scanning"></a>Examen de vulnerabilidades

Azure Security Center proporciona administración unificada de la seguridad y protección avanzada contra amenazas para cargas de trabajo en la nube híbrida. Para Azure Machine Learning, debe habilitar el examen del recurso de Azure Container Registry y los recursos de Azure Kubernetes Service. Consulte [Introducción a Azure Defender para registros de contenedor](../security-center/defender-for-container-registries-introduction.md) e [Introducción a Azure Defender para Kubernetes](../security-center/defender-for-kubernetes-introduction.md).

## <a name="audit-and-manage-compliance"></a>Auditoría y administración del cumplimiento

[Azure Policy](../governance/policy/index.yml) es una herramienta de gobierno que le permite asegurarse de que los recursos de Azure son compatibles con las directivas. Con Azure Machine Learning, puede asignar las siguientes directivas:

* **Clave administrada por el cliente**: audite o exija si las áreas de trabajo deben usar una clave administrada por el cliente.
* **Vínculo privado**: audite si las áreas de trabajo usan un punto de conexión privado para comunicarse con una red virtual.

Para obtener más información sobre Azure Policy, consulte la documentación de [Azure Policy](../governance/policy/overview.md).

Para obtener más información sobre las directivas específicas de Azure Machine Learning, consulte [Auditoría y administración del cumplimiento con Azure Policy](how-to-integrate-azure-policy.md).

## <a name="resource-locks"></a>Bloqueos de recursos

[!INCLUDE [resource locks](../../includes/machine-learning-resource-lock.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Protección de los servicios web Azure Machine Learning con TLS](how-to-secure-web-service.md)
* [Consumir un modelo de Machine Learning implementado como un servicio web](how-to-consume-web-service.md)
* [Uso de Azure Machine Learning con Azure Firewall](how-to-access-azureml-behind-firewall.md)
* [Uso de Azure Machine Learning con Azure Virtual Network](how-to-network-security-overview.md)
* [Cifrado de datos en reposo y en tránsito](concept-data-encryption.md)
* [Compilación de una API de recomendaciones en tiempo real en Azure](/azure/architecture/reference-architectures/ai/real-time-recommendation)

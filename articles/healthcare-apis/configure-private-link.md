---
title: Vínculo privado para Azure API for FHIR
description: En este artículo se describe cómo configurar un punto de conexión privado para los servicios Azure API for FHIR
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 10/12/2020
ms.author: matjazl
ms.openlocfilehash: 121ddcc013fbeef29c6d22cfbd273cfd3b8d3d98
ms.sourcegitcommit: d3c3f2ded72bfcf2f552e635dc4eb4010491eb75
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92558829"
---
# <a name="configure-private-link"></a>Configuración del vínculo privado

> [!IMPORTANT]
> Esta funcionalidad se encuentra en versión preliminar pública, se ofrece sin contrato de nivel de servicio y no se recomienda usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El vínculo privado le permite acceder a Azure API for FHIR a través de un punto de conexión privado, una interfaz de red que le conecta de forma privada y segura mediante una dirección IP privada desde la red virtual. Con el vínculo privado, puede acceder a nuestros servicios de forma segura desde la red virtual como un servicio propietario sin tener que pasar por un DNS público. En este artículo se explica cómo crear, probar y administrar el punto de conexión privado para Azure API for FHIR.

## <a name="prerequisites"></a>Prerrequisitos

Antes de crear un punto de conexión privado, primero tendrá que crear algunos recursos de Azure:

- Grupo de recursos: el grupo de recursos de Azure que contendrá la red virtual y el punto de conexión privado.
- Azure API for FHIR: el recurso FHIR que quiere colocar detrás de un punto de conexión privado.
- Virtual Network: la red virtual a la que se conectarán los servicios de cliente y el punto de conexión privado.

Para obtener más información, consulte la [documentación de Private Link](https://docs.microsoft.com/azure/private-link/).

## <a name="disable-public-network-access"></a>Deshabilitación del acceso a una red pública

La creación de un punto de conexión privado para el recurso FHIR no deshabilita automáticamente el tráfico público que recibe. Para ello, tendrá que actualizar el recurso de FHIR para cambiar la configuración de una nueva propiedad "Acceso público" de "Habilitado" a "Deshabilitado". Tenga cuidado al deshabilitar el acceso a la red pública, ya que se denegarán todas las solicitudes al servicio FHIR que no procedan de un punto de conexión privado configurado correctamente. Solo se permitirá el tráfico desde los puntos de conexión privados.

![Deshabilitación del acceso de la red pública](media/private-link/private-link-disable.png)

## <a name="create-private-endpoint"></a>Creación de un punto de conexión privado

Para crear un punto de conexión privado, un desarrollador con permisos RBAC en el recurso FHIR puede usar Azure Portal, [Azure PowerShell](https://docs.microsoft.com/azure/private-link/create-private-endpoint-powershell)o la [CLI de Azure](https://docs.microsoft.com/azure/private-link/create-private-endpoint-cli). En este artículo se le guía a través de los pasos con Azure Portal. Se recomienda usar Azure Portal, ya que automatiza la creación y configuración de la zona de DNS privado. Para obtener más detalles, puede consultar las [guías de inicio rápido de Private Link](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal).

Hay dos formas de crear un punto de conexión privado. El flujo de aprobación automática permite a un usuario con permisos de RBAC en el recurso FHIR crear un punto de conexión privado sin necesidad de aprobación. El flujo de aprobación manual permite a un usuario sin permisos en el recurso FHIR solicitar un punto de conexión privado para que los propietarios del recurso FHIR lo aprueben.

### <a name="auto-approval"></a>Aprobación automática

Asegúrese de que la región del nuevo punto de conexión privado es la misma que la de la red virtual. La región del recurso FHIR puede ser diferente.

![Pestaña Aspectos básicos de Azure Portal](media/private-link/private-link-portal2.png)

En Tipo de recurso, busque y seleccione "Microsoft.HealthcareApis/services". En Recurso, seleccione el recurso FHIR. Para el subrecurso de destino, seleccione "fhir".

![Pestaña Recursos de Azure Portal](media/private-link/private-link-portal1.png)

Si no tiene una configuración de zona de DNS privado existente, seleccione "(New)privatelink.azurehealthcareapis.com". Si ya ha configurado la zona de DNS privado, puede seleccionarla en la lista. Debe tener el formato "privatelink.azurehealthcareapis.com".

![Pestaña Configuración de Azure Portal](media/private-link/private-link-portal3.png)

Una vez que se ha completado la implementación, puede volver a la pestaña "Conexiones de punto de conexión privado", en la que verá "Aprobado" como estado de la conexión.

### <a name="manual-approval"></a>Aprobación manual

Para la aprobación manual, seleccione la segunda opción en Recurso, "Conéctese a un recurso de Azure por identificador de recurso o alias". En Subrecurso de destino, escriba "fhir" en Aprobación automática.

![Aprobación manual](media/private-link/private-link-manual.png)

Una vez que se ha completado la implementación, puede volver a la pestaña "Conexiones de punto de conexión privado", en la que puede Aprobar, Rechazar o Quitar la conexión.

![Opciones](media/private-link/private-link-options.png)

## <a name="test-private-endpoint"></a>Prueba del punto de conexión privado

Para asegurarse de que el servidor FHIR no recibe tráfico público después de deshabilitar el acceso a la red pública, intente alcanzar el punto de conexión /metadata para el servidor desde el equipo. Debería recibir un mensaje 403 Prohibido. Tenga en cuenta que pueden pasar hasta 5 minutos después de actualizar la marca de acceso a la red pública antes de que se bloquee el tráfico público.

Para asegurarse de que el punto de conexión privado puede enviar tráfico al servidor:

1. Cree una máquina virtual que esté conectada a la red virtual y la subred en la que está configurado el punto de conexión privado. Para asegurarse de que el tráfico de la máquina virtual solo usa la red privada, puede deshabilitar el tráfico saliente de Internet a través de una regla de NSG.
2. Use RDP en la máquina virtual.
3. Intente alcanzar el punto de conexión /metadata del servidor FHIR desde la máquina virtual; debería recibir la instrucción de funcionalidad como respuesta.

## <a name="manage-private-endpoint"></a>Administración de un punto de conexión privado

### <a name="view"></a>Ver

Los puntos de conexión privados y la NIC asociada son visibles en Azure Portal desde el grupo de recursos en el que se han creado.

![Visualización de recursos](media/private-link/private-link-view.png)

### <a name="delete"></a>Eliminar

Los puntos de conexión privados solo se pueden eliminar desde Azure Portal a través de la hoja Información general (como se muestra a continuación), o bien a través de la opción Eliminar de la pestaña "Conexiones de punto de conexión privado" de Redes (versión preliminar). Al hacer clic en el botón Eliminar, se eliminará el punto de conexión privado y la NIC asociada. Si elimina todos los puntos de conexión privados del recurso FHIR y el acceso a la red pública está deshabilitado, no llegará ninguna solicitud al servidor FHIR. Todos los puntos de conexión privados se deben eliminar del recurso FHIR antes de que este se pueda eliminar o mover.

![Eliminación del punto de conexión privado](media/private-link/private-link-delete.png)

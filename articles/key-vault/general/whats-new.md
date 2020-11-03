---
title: Novedades de Azure Key Vault
description: Actualizaciones recientes de Azure Key Vault
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: reference
ms.date: 10/01/2020
ms.author: mbaldwin
ms.openlocfilehash: dbbde397ab235068ea90280da721e3e3dc38866a
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92792368"
---
# <a name="whats-new-for-azure-key-vault"></a>Novedades de Azure Key Vault

Estas son las novedades de Azure Key Vault. También se anuncian nuevas características y mejoras en el [Canal de Key Vault de actualizaciones de Azure](https://azure.microsoft.com/updates/?category=security&query=Key%20vault).

## <a name="october-2020"></a>Octubre de 2020

> [!WARNING]
> Estas actualizaciones pueden afectar a las implementaciones de Azure Key Vault.

Para admitir la [activación predeterminada de la eliminación temporal](#soft-delete-on-by-default), se han realizado dos cambios en los cmdlet de PowerShell de Azure Key Vault:

- Los parámetros DisableSoftDelete y EnableSoftDelete de [Update-AzKeyVault](/powershell/module/az.keyvault/update-azkeyvault) se han dejado de usar.
- La salida del cmdlet [Get-AzKeyVaultSecret](/powershell/module/az.keyvault/get-azkeyvaultsecret) ya no tiene el atributo `SecretValueText`.

## <a name="july-2020"></a>Julio de 2020

> [!WARNING]
> Estas dos actualizaciones pueden afectar a las implementaciones de Azure Key Vault.

### <a name="soft-delete-on-by-default"></a>Activación predeterminada de la eliminación temporal

A finales de 2020, la **eliminación temporal estará activada de forma predeterminada en todos los almacenes de claves** , tanto nuevos como existentes. Para obtener toda la información no solo sobre este posible cambio importante, sino también sobre los pasos necesarios para buscar los almacenes de claves afectados y actualizarlos de antemano, consulte el artículo [La eliminación temporal se habilitará en todos los almacenes de claves](soft-delete-change.md).

### <a name="azure-tls-certificate-changes"></a>Cambios en los certificados TLS de Azure

Microsoft está actualizando los servicios de Azure para que usen los certificados TLS de un conjunto diferente de entidades de certificación (CA) raíz. Este cambio se realiza porque los certificados de entidad de certificación actuales no cumplen uno de los requisitos de la base de referencia del foro CA/Browser.  Para obtener información completa, consulte [Cambios en los certificados TLS de Azure](../../security/fundamentals/tls-certificate-changes.md).

## <a name="june-2020"></a>Junio de 2020

Ahora, Azure Monitor para Key Vault está en versión preliminar.  Azure Monitor proporciona una supervisión completa de los almacenes de claves proporcionando una vista unificada del rendimiento, los errores, la latencia y las solicitudes de Key Vault. Para obtener más información, consulte [Azure Monitor para Key Vault (versión preliminar).](../../azure-monitor/insights/key-vault-insights-overview.md).

## <a name="may-2020"></a>Mayo de 2020

El servicio Bring "Your Own Key" (BYOK) de Key Vault ya está disponible con carácter general. Consulte la [especificación BYOK de Azure Key Vault](../keys/byok-specification.md) y aprenda a [Importar claves protegidas con HSM a Key Vault (BYOK)](../keys/hsm-protected-keys-byok.md).

## <a name="march-2020"></a>Marzo de 2020

Ahora, los puntos de conexión privados están disponibles en versión preliminar. El servicio Azure Private Link permite acceder a Azure Key Vault y a los servicios de asociados o clientes hospedados de Azure mediante un punto de conexión privado de la red virtual.  Aprenda a [integrar Key Vault con Azure Private Link](private-link-service.md).

## <a name="2019"></a>2019

- Versión de los SDK de Azure Key Vault de última generación. Para ver ejemplos de su uso, consulte las guías de inicio rápido de Azure Key Vault para [Python](../secrets/quick-create-python.md), [.NET](../secrets/quick-create-net.md), [Java](../secrets/quick-create-java.md) y [Node.js.](../secrets/quick-create-node.md)
- Nuevas directivas de Azure para administrar certificados de Key Vault. Consulte las [definiciones integradas de Azure Policy para Key Vault](../policy-reference.md).
- La extensión de máquina virtual de Azure Key Vault ya está disponible con carácter general.  Consulte [extensión de máquina virtual de Key Vault para Linux](../../virtual-machines/extensions/key-vault-linux.md) y [extensión de máquina virtual de Key Vault para Windows](../../virtual-machines/extensions/key-vault-windows.md).
- Administración de secretos basada en eventos para Azure Key Vault ya disponible en Azure Event Grid. Para obtener más información, consulte [el esquema de Event Grid para eventos en Azure Key Vault](../../event-grid/event-schema-key-vault.md) y aprenda a [recibir y responder notificaciones del almacén de claves con Azure Event Grid](event-grid-tutorial.md).

## <a name="2018"></a>2018

Nuevas características e integraciones lanzadas este año:

- Integración con Azure Functions. Para ver un escenario de ejemplo en el que se aprovecha [Azure Functions](../../azure-functions/index.yml) para las operaciones del almacén de claves, consulte [Automatización de la rotación de secretos](../secrets/tutorial-rotation.md).
- [Integración con Azure Databricks](/azure/databricks/scenarios/store-secrets-azure-key-vault). Con esto, ahora, Azure Databricks admite dos tipos de ámbitos secretos: basado en Azure Key Vault y en Databricks. Para obtener más información, consulte [Creación de un ámbito de secreto basado en Azure Key Vault](/azure/databricks/security/secrets/secret-scopes#--create-an-azure-key-vault-backed-secret-scope)
- [Puntos de conexión de servicio de red virtual para Azure Key Vault](overview-vnet-service-endpoints.md).

## <a name="2016"></a>2016

Nuevas características publicadas este año:

- Claves de cuenta de almacenamiento administrada. La característica Claves de cuenta de almacenamiento supuso una integración más sencilla con Azure Storage. Para más información, consulte el tema de introducción, [Claves de cuenta de almacenamiento de Azure Key Vault](../secrets/overview-storage-keys.md).
- Eliminación temporal. La característica de eliminación temporal mejora la protección de los datos de los almacenes de claves y de los objetos de estos. Para más información, consulte el tema de introducción [Información general sobre la eliminación temporal de Azure Key Vault](./soft-delete-overview.md).

## <a name="2015"></a>2015

Nuevas características publicadas este año:
- Administración de certificados. Se agregó como característica a la versión 2015-06-01 de disponibilidad general el 26 de septiembre de 2016.

La versión 2015-06-01 de disponibilidad general se anunció el 24 de junio de 2015. Se han realizado los siguientes cambios en esta versión:
- Eliminación de una clave: se ha quitado el campo "uso".
- Obtención de información acerca de una clave: se ha quitado el campo "uso".
- Importación de una clave en un almacén: se ha quitado el campo "uso".
- Restauración de una clave: se ha quitado el campo "uso".
- Se ha cambiado "RSA_OAEP" a "RSA-OAEP" para los algoritmos RSA. Consulte [Información acerca de claves, secretos y certificados](about-keys-secrets-certificates.md).

La segunda versión preliminar 2015-02-01-preview se anunció el 20 de abril de 2015. Para más información, consulte la entrada de blog [REST API Update](/archive/blogs/kv/rest-api-update) (Actualización de API REST). Se actualizaron las siguientes tareas:

- Enumeración de las claves en un almacén: se ha agregado compatibilidad con la paginación a la operación.
- Enumeración de las versiones de una clave: se ha agregado una operación para enumerar las versiones de una clave.
- Enumeración de los secretos de un almacén: se ha agregado compatibilidad con la paginación.
- Enumeración de las versiones de un secreto: se ha agregado una operación para enumerar las versiones de un secreto.
- Todas las operaciones: se han agregado marcas de tiempo de creación o actualización a los atributos.
- Creación de un secreto: se ha agregado Content-Type a los secretos.
- Creación de una clave: se han agregado etiquetas como información opcional.
- Creación de un secreto: se han agregado etiquetas como información opcional.
- Actualización de una clave: se han agregado etiquetas como información opcional.
- Actualización de un secreto: se han agregado etiquetas como información opcional.
- Se ha cambiado el tamaño máximo de los secretos de 10 KB a 25 KB. Consulte [Información acerca de claves, secretos y certificados](about-keys-secrets-certificates.md).

## <a name="2014"></a>2014

La primera versión preliminar 2014-12-08-preview se anunció el 8 de enero de 2015.

## <a name="next-steps"></a>Pasos siguientes

Si tiene más preguntas, póngase en contacto con nosotros a través del departamento de [soporte técnico](https://azure.microsoft.com/support/options/).
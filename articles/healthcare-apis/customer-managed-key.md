---
title: Configuración de claves administradas por el cliente Azure API for FHIR
description: Incorpore su propia característica clave compatible con Azure API for FHIR mediante Cosmos DB
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: overview
ms.date: 09/28/2020
ms.author: matjazl
ms.openlocfilehash: 05c208ba3c9005d38b8924037748764f8d112e3a
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93398188"
---
# <a name="configure-customer-managed-keys"></a>Configuración de claves administradas por el cliente

Cuando crea una cuenta de Azure API for FHIR, de forma predeterminada los datos se cifran mediante claves administradas por Microsoft. Ahora puede agregar una segunda capa de cifrado de datos mediante una clave que usted mismo elige y administra.

En Azure, para hacerlo se suele utilizar una clave de cifrado en la instancia de Azure Key Vault (AKV) del cliente. Azure SQL, Azure Storage y Cosmos DB son algunos ejemplos que proporcionan esta funcionalidad hoy en día. Azure API for FHIR aprovecha esta compatibilidad con Cosmos DB. Al crear una cuenta, tendrá la opción de especificar el identificador URI de una clave de Azure Key Vault. Esta clave se pasará a Cosmos DB al aprovisionar la cuenta de la base de datos. Cuando se realiza una solicitud de FHIR, Cosmos DB captura su clave y la usa para cifrar o descifrar los datos. Para empezar, puede consultar los vínculos siguientes:

- [Registro del proveedor de recursos de Azure Cosmos DB para su suscripción a Azure](../cosmos-db/how-to-setup-cmk.md#register-resource-provider) 
- [Configuración de la instancia de Azure Key Vault](../cosmos-db/how-to-setup-cmk.md#configure-your-azure-key-vault-instance)
-  [Adición de una directiva de acceso a la instancia de Azure Key Vault](../cosmos-db/how-to-setup-cmk.md#add-an-access-policy-to-your-azure-key-vault-instance)
- [Generación de una clave en Azure Key Vault](../cosmos-db/how-to-setup-cmk.md#generate-a-key-in-azure-key-vault)

Después de crear la cuenta Azure API for FHIR en Azure Portal, puede ver la opción de configuración "Cifrado de datos" en la sección "Configuración de base de datos" en la pestaña "Configuración adicional". De forma predeterminada, se elegirá la opción de clave administrada por el servicio. Para especificar la clave de Azure Key Vault, seleccione la opción "Clave administrada por el cliente". Aquí puede escribir el identificador URI de la clave copiada.

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-create.png" alt-text="Creación de Azure API for FHIR":::

O bien, puede elegir un clave en KeyPicker:

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-keypicker.png" alt-text="KeyPicker":::

En el caso de las cuentas de FHIR existentes, puede ver la opción de cifrado de clave (clave administrada por el cliente o por el servicio) en la hoja "Base de datos" como se indica a continuación. La opción de configuración no se puede modificar una vez elegida. Sin embargo, se puede modificar y actualizar la clave.

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-database.png" alt-text="Base de datos":::

Además, puede crear una nueva versión de la clave especificada, después de la cual los datos se cifrarán con la nueva versión sin ninguna interrupción del servicio. También puede quitar el acceso a la clave para quitar el acceso a los datos.
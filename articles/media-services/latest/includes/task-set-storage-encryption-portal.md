---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 11/04/2020
ms.author: inhenkel
ms.custom: portal
ms.openlocfilehash: c3a27218f03980e9b42a4a56fa739203c43754a0
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "96013282"
---
<!--Set the encryption on storage account in the portal-->

## <a name="set-the-encryption-on-a-storage-account"></a>Establecimiento del cifrado en una cuenta de almacenamiento

1. En Azure Portal, escriba el nombre de la cuenta de almacenamiento que desea cifrar en el campo **Buscar** en la parte superior de la pantalla.  Las coincidencias aparecerán debajo del campo de búsqueda.
1. Seleccione la cuenta de almacenamiento que busca. Aparecerá la pantalla de la cuenta de almacenamiento.
1. Seleccione **Cifrado**.
1. Seleccione Claves administradas por Microsoft o Claves administradas por el cliente.

### <a name="use-microsoft-managed-keys"></a>Uso de claves administradas por Microsoft

De manera predeterminada, los datos de la cuenta de almacenamiento se cifran con claves administradas por Microsoft.

### <a name="use-customer-managed-keys"></a>Uso de claves administradas por el cliente

1. Select **Claves administradas por el cliente**.
1. Seleccione **Escribir el URI de la clave** o **Seleccionar de Key Vault**.
    1. Si selecciona **Escribir el URI de la clave**, escriba el identificador URI de la clave en el campo Identificador URI de la clave y seleccione la suscripción. (Puede que ya esté seleccionada).
    1. Si selecciona **Seleccionar de Key Vault**, seleccione a continuación **Seleccionar un almacén de claves y una clave**. Aparecerá la pantalla Seleccionar clave desde Azure Key Vault.
1. Seleccione el **almacén de claves** que desea usar y seleccione una clave que ya tenga en el almacén de claves o **Crear una nueva clave**.
    1. Si decide crear una nueva clave, seleccione **Generar** o **Importar** desde el desplegable **Opciones**. Solo puede importar claves RSA.
    1. Para generar una nueva clave, asigne un nombre a la clave en el campo **Nombre** y seleccione el tipo de clave:
        1. Tamaños de clave de RSA:  2048, 3072 o 4096. Esto es lo que la mayoría de los clientes eligen.
        1. EC: nombres de curva elíptica: P-256, P-384, P-521 o P-256K
        1. Opcionalmente, puede establecer las fechas de activación y expiración de la clave.
        1. Seleccione **Sí** para habilitar la rotación de claves automática.
        1. Seleccione **Crear**.
    1. Para importar una clave, seleccione el archivo que se va a cargar mediante un clic en **Seleccionar un campo de archivo**.
        1. Asigne un nombre a la clave en el campo **Nombre**.
        1. Opcionalmente, puede establecer las fechas de activación y expiración de la clave.
        1. Seleccione **Sí** para habilitar la rotación de claves automática.
        1. Seleccione **Crear**.
    1. Seleccione **Seleccionar** para seleccionar esta clave para cifrar la cuenta de almacenamiento. Volverá a la pantalla de cifrado.
1. **IMPORTANTE:** Seleccione **Guardar** para guardar la configuración de cifrado o se perderán todos los cambios que acaba de hacer.

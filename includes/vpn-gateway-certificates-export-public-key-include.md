---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/19/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f71703dcb6afb5ce7610bf1c5374de1cc98816b0
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606918"
---
Después de crear un certificado raíz autofirmado, exporte el archivo .cer de clave pública de certificado de raíz (no la clave privada). Este archivo se cargará más adelante en Azure. Los pasos siguientes le ayudan a exportar el archivo .cer para el certificado raíz autofirmado:

1. Para obtener un archivo .cer del certificado, abra **Administrar certificados de usuario**. Busque el certificado raíz autofirmado; normalmente se encuentra en Certificados - Usuario actual\Personal\Certificados y haga clic en el botón derecho. Haga clic en **Todas las tareas** y, luego, en **Exportar**. Se abre el **Asistente para exportar certificados**. Si no encuentra el certificado en Usuario actual\Personal\Certificados, puede que haya abierto de forma accidental "Certificados – equipo Local", en lugar de "Certificados - Usuario actual"). Si desea abrir el Administrador de certificados en el ámbito del usuario actual mediante PowerShell, escriba *certmgr* en la ventana de la consola.

   ![Captura de pantalla que muestra la ventana Certificados del usuario actual con Certificados seleccionado y un menú contextual con Todas las tareas y, a continuación, Exportar seleccionados.](./media/vpn-gateway-certificates-export-public-key-include/export.png)
2. En el asistente, haga clic en **Siguiente**.

   ![Exportación de certificado](./media/vpn-gateway-certificates-export-public-key-include/exportwizard.png)
3. Seleccione **No exportar la clave privada** y, después, haga clic en **Siguiente**.

   ![No exportar la clave privada](./media/vpn-gateway-certificates-export-public-key-include/notprivatekey.png)
4. En la página **Formato de archivo de exportación**, seleccione **X.509 codificado base 64 (.CER)** y, luego, haga clic en **Siguiente**.

   ![Codificado con Base 64](./media/vpn-gateway-certificates-export-public-key-include/base64.png)
5. En **Archivo que se va a exportar**, haga clic en **Examinar** para ir a la ubicación a la que desea exportar el certificado. En **Nombre de archivo**, asígnele un nombre al archivo de certificado. A continuación, haga clic en **Siguiente**.

   ![Captura de pantalla que muestra el Asistente para exportar certificados con un cuadro de texto Nombre de archivo y una opción Examinar.](./media/vpn-gateway-certificates-export-public-key-include/browse.png)
6. Haga clic en **Finalizar** para exportar el certificado.

   ![Captura de pantalla que muestra la página final del Asistente para exportar certificados con la configuración seleccionada.](./media/vpn-gateway-certificates-export-public-key-include/finish.png)
7. El certificado se exportó correctamente.

   ![Captura de pantalla que muestra un mensaje que indica que la exportación se realizó correctamente.](./media/vpn-gateway-certificates-export-public-key-include/success.png)
8. El certificado exportado tiene un aspecto similar al siguiente:

   ![Captura de pantalla que muestra un icono de certificado y un nombre de archivo con la extensión de nombre de archivo c e r.](./media/vpn-gateway-certificates-export-public-key-include/exported.png)
9. Si abre el certificado exportado mediante el Bloc de notas, verá algo parecido a este ejemplo. La sección en azul contiene la información que se carga en Azure. Si abre el certificado con el Bloc de notas y no se parece a este, normalmente eso significa que no lo exportó mediante el formato X.509 codificado base 64 (. CER). Además, si desea utilizar un editor de texto diferente, debe comprender que algunos editores pueden introducir formatos no deseados en segundo plano. Esto puede crear problemas al cargar el texto de este certificado en Azure.

   ![Abrir con el Bloc de notas](./media/vpn-gateway-certificates-export-public-key-include/notepad.png)

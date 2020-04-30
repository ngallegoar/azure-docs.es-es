---
title: Certificar la imagen de VM para Azure Marketplace
description: Se explica cómo probar y enviar una imagen de máquina virtual para la certificación de Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/26/2018
ms.author: dsindona
ms.openlocfilehash: 81dec159a8a84adce97750666baba455ae3fdd93
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82147074"
---
# <a name="certify-your-vm-image"></a>Certificar la imagen de máquina virtual

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos el traslado de la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Certificación de imágenes de máquina virtual de Azure](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-image-certification) para administrar las ofertas migradas.

Después de crear e implementar la máquina virtual (VM), debe probar y enviar la imagen de esta para la certificación de Azure Marketplace. En este artículo se explica dónde obtener *Certification Test Tool for Azure Certified*, cómo usar esta herramienta para certificar la imagen de máquina virtual y cómo cargar los resultados de verificación en el contenedor de Azure donde residen los VHD. 


## <a name="download-and-run-the-certification-test-tool"></a>Descargar y ejecutar Certification Test Tool

Certification Test Tool for Azure Certified se ejecuta en un equipo Windows local, pero prueba una máquina virtual Linux o Windows basada en Azure.  Verifica que la imagen de máquina virtual de usuario es compatible con Microsoft Azure: que se han cumplido las instrucciones y los requisitos relacionados con la preparación del VHD. La salida de la herramienta es un informe de compatibilidad que se va a cargar en [Cloud Partner Portal](https://cloudpartner.azure.com) para solicitar la certificación de máquina virtual.

1. Descargue e instale la versión más reciente de [Certification Test Tool for Azure Certified](https://www.microsoft.com/download/details.aspx?id=44299). 
2. Abra la herramienta y haga clic en **Iniciar nueva prueba**.
3. En la pantalla **Información de la prueba**, escriba un **Nombre de prueba** para la serie de pruebas.
4. Seleccione la **Plataforma** de la máquina virtual, ya sea `Windows Server` o `Linux`. La elección de plataforma afecta a las opciones restantes.
5. Si la máquina virtual usa este servicio de base de datos, active la casilla **Prueba de Azure SQL Database**.

   ![Página inicial de la herramienta de certificación](./media/publishvm_025.png)


## <a name="connect-the-certification-tool-to-a-vm-image"></a>Conectar la herramienta de certificación a una imagen de máquina virtual

  La herramienta se conecta a máquinas virtuales basadas en Windows con [PowerShell](https://docs.microsoft.com/powershell/) y a máquinas virtuales basadas en Linux mediante [SSH.Net](https://www.ssh.com/ssh/protocol/).

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a>Conectar la herramienta de certificación a una imagen de máquina virtual Linux

1. Seleccione el modo de **Autenticación de SSH**: `Password Authentication` o `key File Authentication`.
2. Si usa autenticación por contraseña, escriba valores para **Nombre DNS de máquina virtual**, **Nombre de usuario** y **Contraseña**.  Opcionalmente, puede cambiar el número predeterminado de **Puerto SSH**.

     ![Autenticación de contraseña de imagen de máquina virtual de Linux](./media/publishvm_026.png)

3. Si usa autenticación con archivo de clave, escriba valores para **Nombre DNS de máquina virtual**, **Nombre de usuario** y **Ubicación de clave privada**.  Opcionalmente, puede proporcionar una **Frase de contraseña** o cambiar el número predeterminado de **Puerto SSH**.

     ![Autenticación de archivo de imagen de máquina virtual Linux](./media/publishvm_027.png)

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a>**Conexión de la herramienta de certificación a una imagen de máquina virtual basada en Windows**
1. Escriba el **Nombre DNS de máquina virtual** completo (por ejemplo, `MyVMName.Cloudapp.net`).
2. Escriba valores para **Nombre de usuario** y **Contraseña**.

   ![Autenticación de contraseña de imagen de máquina virtual de Windows](./media/publishvm_028.png)


## <a name="run-a-certification-test"></a>Ejecutar una prueba de certificación

Una vez proporcionados los valores de parámetro para la imagen de máquina virtual en la herramienta de certificación, seleccione **Probar conexión** para garantizar una conexión válida a la máquina virtual. Una vez verificada la conexión, seleccione **Siguiente** para iniciar la prueba.  Una vez completada la prueba, aparece una tabla con los resultados (Correcto, Error o Advertencia).  En el ejemplo siguiente se muestran los resultados de una prueba de máquina virtual Linux. 

![Resultados de prueba de certificación de imagen de máquina virtual Linux](./media/publishvm_029.png)

Si se produce un error en alguna de las pruebas, la imagen *no* se certifica. En ese caso, revise los requisitos y los mensajes de error, realice los cambios indicados y vuelva a ejecutar la prueba. 

Después de la prueba automatizada, se le pide que proporcione información adicional sobre la imagen de máquina virtual en la pantalla **Cuestionario**.  Contiene dos pestañas que debe completar.  La pestaña **Evaluación general** contiene preguntas **verdadero o falso**, mientras que **Personalización del kernel** contiene varias preguntas de selección múltiple y respuesta libre.  Complete las preguntas de ambas pestañas y seleccione **Siguiente**.

![Cuestionario de la herramienta de certificación](./media/publishvm_030.png)

La última pantalla permite proporcionar información adicional, como datos de acceso SSH para una imagen de máquina virtual Linux y una explicación sobre las evaluaciones con errores, en caso de que busque excepciones. 

Por último, haga clic en **Generar informe** para descargar los resultados de las pruebas y los archivos de registro de las pruebas ejecutadas, además de las respuestas al cuestionario. Guarde los resultados en el mismo contenedor que los VHD.

![Guardar resultados de la prueba de certificación](./media/publishvm_031.png)


## <a name="next-steps"></a>Pasos siguientes

Luego debe [generar identificadores uniformes de recursos (URI) para cada VHD](./cpp-get-sas-uri.md) que envíe a Marketplace. 

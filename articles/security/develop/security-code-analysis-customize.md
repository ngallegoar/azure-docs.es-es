---
title: Personalización de las tareas de análisis de código de seguridad de Microsoft
titleSuffix: Azure
description: En este artículo se describe la personalización de las tareas en la extensión Análisis de código de seguridad de Microsoft
author: vharindra
manager: sukhans
ms.author: terrylan
ms.date: 07/31/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 6cdf892651407defc21f359a8e3b326b4af63b62
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77499999"
---
# <a name="configure-and-customize-the-build-tasks"></a>Configuración y personalización de las tareas de compilación

En este artículo se describen de forma detallada las opciones de configuración disponibles en cada una de las tareas de compilación. El artículo se inicia con las tareas para las herramientas de análisis de código de seguridad. Finaliza con las tareas posteriores al procesamiento.

## <a name="anti-malware-scanner-task"></a>Tarea de Anti-Malware Scanner

>[!NOTE]
> La tarea de compilación Anti-Malware Scanner requiere un agente de compilación con Windows Defender habilitado. Tanto Visual Studio 2017 hospedado como las versiones posteriores proporcionan ese agente. La tarea de compilación no se ejecutará en el agente hospedado de Visual Studio 2015.
>
> Aunque las firmas no se pueden actualizar en estos agentes, siempre deben tener una antigüedad inferior a tres horas.

En la captura de pantalla y el texto siguientes se muestran detalles de la configuración de tareas.

![Configuración de la tarea de compilación de Anti-Malware Scanner](./media/security-tools/5-add-anti-malware-task600.png)

En el cuadro de lista **Tipo** de la captura de pantalla, se selecciona **Básico**. Seleccione **Personalizado** para proporcionar argumentos de la línea de comandos que personalicen el examen.

Windows Defender usa el cliente de Windows Update para descargar e instalar firmas. Si se produce un error en la actualización de firmas en el agente de compilación, es probable que el código de error **HRESULT** proceda de Windows Update.

Para obtener más información sobre los errores de Windows Update y su mitigación, consulte [Códigos de error de Windows Update por componente](https://docs.microsoft.com/windows/deployment/update/windows-update-error-reference) y el artículo de TechNet sobre los [códigos de error del agente de Windows Update](https://social.technet.microsoft.com/wiki/contents/articles/15260.windows-update-agent-error-codes.aspx).

Para obtener información sobre la configuración de YAML para esta tarea, consulte nuestras [opciones de antimalware de YAML](yaml-configuration.md#anti-malware-scanner-task).

## <a name="binskim-task"></a>Tarea de BinSkim

> [!NOTE]
> Para poder ejecutar la tarea de BinSkim, su compilación debe cumplir una de estas condiciones:
>  - La compilación genera artefactos binarios a partir de código administrado.
>  - Se han confirmado artefactos binarios que desea analizar con BinSkim.

En la captura de pantalla y la lista siguientes se muestran detalles de la configuración de tareas.

![Configuración de la tarea de compilación de BinSkim](./media/security-tools/7-binskim-task-details.png)

- Establezca la configuración de compilación en Depurar para generar archivos de depuración .pdb. BinSkim usa estos archivos para volver a asignar problemas de los archivos binarios de salida al código fuente.
- Para evitar la búsqueda y creación de su propia línea de comandos:
     - En la lista **Tipo**, seleccione **Básico**.
     - En la lista **Función**, seleccione **Analizar**.
- En **Destino**, escriba uno o varios especificadores para un archivo, directorio o patrón del filtro. Estos especificadores se resuelven en uno o varios archivos binarios que se van a analizar:
    - Cuando se agregan varios destinos especificados, deben separarse con un punto y coma (;).
    - Un especificador puede ser un único archivo o contener caracteres comodín.
    - Las especificaciones del directorio siempre deben terminar con \\*.
    - Ejemplos:

           *.dll;*.exe
           $(BUILD_STAGINGDIRECTORY)\*
           $(BUILD_STAGINGDIRECTORY)\*.dll;$(BUILD_STAGINGDIRECTORY)\*.exe;

- Si selecciona **Línea de comandos** en la lista **Tipo**, debe ejecutar binskim.exe:
     - Asegúrese de que los primeros argumentos de binskim.exe son el verbo **analizar** seguido de una o varias especificaciones de ruta de acceso. Cada ruta de acceso puede ser una ruta de acceso completa o una ruta de acceso relativa al directorio de origen.
     - Varias rutas de acceso de destino deben separarse con un espacio.
     - Puede omitir la opción **/o** o **/output**. El valor de salida se agrega automáticamente o se reemplaza.
     - Las configuraciones de línea de comandos se muestran de la siguiente manera.

           analyze $(Build.StagingDirectory)\* --recurse --verbose
           analyze *.dll *.exe --recurse --verbose

          > [!NOTE]
          > El signo \\* final es importante si especifica los directorios para el destino.

Para obtener más información sobre los argumentos de la línea de comandos de BinSkim, las reglas por identificador o los códigos de salida, consulte la [guía de usuario de BinSkim](https://github.com/Microsoft/binskim/blob/master/docs/UserGuide.md).

Para obtener información sobre la configuración de YAML para esta tarea, consulte nuestras [opciones de BinSkim de YAML](yaml-configuration.md#binskim-task).

## <a name="credential-scanner-task"></a>Tarea de Credential Scanner

En la captura de pantalla y la lista siguientes se muestran detalles de la configuración de tareas.

![Configuración de la tarea de compilación de Credential Scanner](./media/security-tools/3-taskdetails.png)

Las opciones disponibles incluyen:

  - **Formato de salida**: entre los valores disponibles se incluyen **TSV**, **CSV**, **SARIF** y **PREfast**.
  - **Versión de la herramienta**: recomendamos que seleccione **Más reciente**.
  - **Examinar carpeta**: carpeta del repositorio que se va a examinar.
  - **Tipo de archivo del buscador**: opciones para encontrar el archivo de buscador utilizado para el examen.
  - **Archivo de eliminaciones**: un archivo [JSON](https://json.org/) puede eliminar los problemas en el registro de salida. Para obtener más información sobre los escenarios de eliminación, consulte la sección P+F de este artículo.
  - **Resultado detallado**: se explica por sí solo.
  - **Tamaño de lote**: el número de subprocesos simultáneos que se usan para ejecutar Credential Scanner. El valor predeterminado es 20. Los valores posibles oscilan entre 1 y 2 147 483 647.
  - **Tiempo de espera de coincidencia**: la cantidad de tiempo en segundos que se invertirá en encontrar un buscador antes de detener la comprobación.
  - **Tamaño de búfer de lectura para el examen de archivos**: tamaño en bytes del búfer que se usa durante la lectura de contenido. El valor predeterminado es 524 288.  
  - **Bytes de lectura máximos para el examen de archivos**: número máximo de bytes que van a leerse en un archivo durante un análisis de contenido. El valor predeterminado es 104 857 600.
  - **Opciones de control** > **Ejecutar esta tarea**: especifica cuándo se ejecutará la tarea. Seleccione **Condiciones personalizadas** para especificar condiciones más complejas.
  - **Versión**: versión de la tarea de compilación en Azure DevOps. Esta opción no se usa con frecuencia.

Para obtener información sobre la configuración de YAML para esta tarea, consulte nuestras [opciones del detector de credenciales (CredScan) de YAML](yaml-configuration.md#credential-scanner-task).

## <a name="microsoft-security-risk-detection-task"></a>Tarea de Microsoft Security Risk Detection

> [!NOTE]
> Debe crear y configurar una cuenta con el servicio Microsoft Security Risk Detection (MSRD) antes de usar la tarea de MSRD. Este servicio requiere un proceso de incorporación independiente. A diferencia de la mayor parte de las otras tareas de esta extensión, esta tarea requiere una suscripción independiente con MSRD.
>
> Consulte [Microsoft Security Risk Detection](https://aka.ms/msrddocs) y [Microsoft Security Risk Detection: How To](https://docs.microsoft.com/security-risk-detection/how-to/) (Microsoft Security Risk Detection: instrucciones) para obtener instrucciones.

En la siguiente lista se muestran detalles para la configuración de esta tarea. Para cualquier elemento de interfaz de usuario, puede mantener el mouse sobre ese elemento para obtener ayuda.

   - **Azure DevOps Service Endpoint Name for MSRD** (nombre del punto de conexión de servicio de Azure DevOps para MSRD): un tipo genérico de punto de conexión de servicio de Azure DevOps almacena la dirección URL de la instancia de MSRD incorporada y el token de acceso de la API de REST. Si ha creado dicho punto de conexión, puede especificarlo aquí. De lo contrario, seleccione el vínculo **Administrar** para crear y configurar un nuevo punto de conexión de servicio para esta tarea de MSRD.
   - **Id. de cuenta**: un GUID que se puede recuperar de la dirección URL de la cuenta de MSRD.
   - **URLs to Binaries** (direcciones URL para archivos binarios): una lista delimitada por punto y coma de las direcciones URL disponibles públicamente. La máquina de pruebas de vulnerabilidad usa estas direcciones URL para descargar los archivos binarios.
   - **URLs of the Seed Files** (direcciones URL de los archivos de valores iniciales): una lista delimitada por punto y coma de las direcciones URL disponibles públicamente. La máquina de pruebas de vulnerabilidad usa estas direcciones URL para descargar los archivos binarios. Especificar este valor es opcional si se descargan los archivos de valores iniciales junto con los archivos binarios.
   - **OS Platform Type** (tipo de plataforma de SO): la plataforma del sistema operativo (SO) de las máquinas que ejecutan el trabajo de pruebas de vulnerabilidad. Los valores disponibles son **Windows** y **Linux**.
   - **Windows Edition / Linux Edition** (edición de Windows o Linux): edición del sistema operativo de las máquinas que ejecutan el trabajo de pruebas de vulnerabilidad. Puede sobrescribir el valor predeterminado si sus máquinas tienen una edición de sistema operativo diferente.
   - **Package Installation Script** (script de instalación del paquete): script que se va a ejecutar en una máquina de prueba. Este script instala el programa de destino de la prueba y sus dependencias antes de enviarse el trabajo de pruebas de vulnerabilidad.
   - **Job Submission Parameters** (parámetros de envío del trabajo):
       - **Seed Directory** (directorio de valores iniciales): ruta de acceso al directorio en la máquina de pruebas de vulnerabilidad que contiene los valores iniciales.
       - **Seed Extension** (extensión de los valores iniciales): la extensión de nombre de archivo de los valores iniciales.
       - **Test Driver Executable** (ejecutable del controlador de prueba): ruta de acceso al archivo ejecutable de destino en la máquina de pruebas de vulnerabilidad.
       - **Test Driver Executable Architecture** (arquitectura del ejecutable del controlador de prueba): arquitectura del archivo ejecutable de destino. Los valores disponibles son **x86** y **amd64**.
       - **Test Driver Arguments** (argumentos del controlador de prueba): argumentos de la línea de comandos pasados al archivo ejecutable de la prueba. El argumento "%testfile%", incluidas las comillas, se reemplaza automáticamente por la ruta de acceso completa al archivo de destino. El controlador de prueba analiza este archivo, el cual es necesario.
       - **Test Driver Process Exits Upon Test Completion** (el proceso del controlador de prueba finaliza al completarse la prueba): active esta casilla si el controlador de prueba se va a terminar tras la finalización. Desactívela si el controlador de prueba tiene que cerrarse a la fuerza.
       - **Duración máxima (en segundos)** : una estimación del mayor tiempo que cabe esperar para que el programa de destino analice un archivo de entrada. Cuanto más precisa sea la estimación, mayor será la eficacia de la ejecución de la aplicación de pruebas de vulnerabilidad.
       - **Test Driver Can Be Run Repeatedly** (el controlador de prueba puede ejecutarse repetidamente): active esta casilla si el controlador de prueba se puede ejecutar repetidamente sin depender de un estado global persistente o compartido.
       - **Test Driver Can Be Renamed** (se puede cambiar el nombre del controlador de prueba): active esta casilla si al cambiar el nombre del archivo ejecutable del controlador de prueba, este puede seguir funcionando correctamente.
       - **The Fuzzing Application Runs as a Single OS Process** (la aplicación de pruebas de vulnerabilidad se ejecuta como un proceso de sistema operativo único): active esta casilla si el controlador de prueba se ejecuta en un único proceso del sistema operativo. Desactívela si el controlador de prueba genera procesos adicionales.

Para obtener información sobre la configuración de YAML para esta tarea, consulte nuestras [opciones de detección de riesgos de seguridad de Microsoft de YAML](yaml-configuration.md#microsoft-security-risk-detection-task).

## <a name="roslyn-analyzers-task"></a>Tarea de analizadores basados en Roslyn

> [!NOTE]
> Antes de poder ejecutar la tarea de analizadores basados en Roslyn, su compilación debe cumplir estas condiciones:
>
> - La definición de compilación incluye la tarea de compilación integrada de MSBuild o VSBuild para compilar código de C# o Visual Basic. La tarea de analizadores se basa en la entrada y la salida de la tarea integrada para ejecutar la compilación de MSBuild con los analizadores basados en Roslyn habilitados.
> - El agente de compilación que ejecuta esta tarea de compilación tiene instalado Visual Studio 2017 versión 15.5 o posterior, por lo que usa la versión 2.6 o posterior del compilador.

En la lista y nota siguientes se muestran detalles de la configuración de tareas.

Las opciones disponibles incluyen:

- **Conjunto de reglas**: los valores son **SDL Required** (SDL requerido), **SDL Recommended** (SDL recomendado) o su propio conjunto de reglas personalizado.
- **Versión del analizador**: recomendamos que seleccione **Más reciente**.
- **Archivo de eliminaciones de advertencias del compilador**: un archivo de texto con una lista de identificadores de advertencias que se eliminan.
- **Opciones de control** > **Ejecutar esta tarea**: especifica cuándo se ejecutará la tarea. Elija **Condiciones personalizadas** para especificar condiciones más complejas.

> [!NOTE]
>
> - Los analizadores basados en Roslyn están integrados con el compilador y solo se pueden ejecutar como parte de la compilación de csc.exe. Por lo tanto, esta tarea requiere que se vuelva a reproducir o ejecutar el comando del compilador que se ejecutó anteriormente en la compilación. Esta reproducción o ejecución se realiza consultando Visual Studio Team Services (VSTS) para los registros de tareas de compilación de MSBuild.
>
>   No hay ninguna otra manera de que la tarea obtenga de forma confiable la línea de comandos de compilación de MSBuild de la definición de compilación. Consideramos la posibilidad de agregar un cuadro de texto de forma libre para permitir a los usuarios escribir sus líneas de comandos. Sin embargo, sería difícil en ese caso mantener esas líneas de comandos actualizadas y sincronizadas con la compilación principal.
>
>   Las compilaciones personalizadas requieren que se vuelva a reproducir todo el conjunto de comandos, no solo los comandos del compilador. En estos casos, habilitar los analizadores basados en Roslyn no es trivial ni confiable.
>
> - Los analizadores basados en Roslyn están integrados con el compilador. Para su invocación, los analizadores basados en Roslyn requieren compilación.
>
>   Esta nueva tarea de compilación se implementa recompilando proyectos de C# que ya se han compilado. La nueva tarea solo usa las tareas de compilación de MSBuild y VSBuild en la misma compilación o definición de compilación que la tarea original. Sin embargo, en este caso, la nueva tarea las usa con los analizadores basados en Roslyn habilitados.
>
>   Si la nueva tarea se ejecuta en el mismo agente que la tarea original, el resultado de la nueva tarea sobrescribe el resultado de la tarea original en la carpeta de orígenes *s*. Aunque el resultado de la compilación es el mismo, se recomienda ejecutar MSBuild, copiar los resultados en el directorio de almacenamiento provisional de los artefactos y, después, ejecutar los analizadores basados en Roslyn.

Para obtener recursos adicionales para la tarea del analizador basado en Roslyn, consulte los [analizadores basados en Roslyn](https://docs.microsoft.com/dotnet/standard/analyzers/) en Microsoft Docs.

Encontrará el paquete de analizadores instalado y usado por esta tarea de compilación en la página de NuGet [Microsoft.CodeAnalysis.FxCopAnalyzers](https://www.nuget.org/packages/Microsoft.CodeAnalysis.FxCopAnalyzers).

Para obtener información sobre la configuración de YAML para esta tarea, consulte nuestras [opciones de analizadores de Roslyn de YAML](yaml-configuration.md#roslyn-analyzers-task).

## <a name="tslint-task"></a>Tarea de TSLint

Para obtener más información sobre TSLint, vaya al [repositorio de GitHub de TSLint](https://github.com/palantir/tslint).

>[!NOTE] 
>Como tal vez sepa, en la página principal del [repositorio de GitHub de TSLint](https://github.com/palantir/tslint) se indica que TSLint quedará obsoleto en algún momento de 2019. Microsoft investiga [ESLint](https://github.com/eslint/eslint) como tarea alternativa.

Para obtener información sobre la configuración de YAML para esta tarea, consulte nuestras [opciones de TSLint de YAML](yaml-configuration.md#tslint-task).

## <a name="publish-security-analysis-logs-task"></a>Tarea de Publish Security Analysis Logs

En la captura de pantalla y la lista siguientes se muestran detalles de la configuración de tareas.

![Configuración de la tarea de compilación de Publish Security Analysis Logs](./media/security-tools/9-publish-security-analsis-logs600.png)  

- **Nombre del artefacto**: cualquier identificador de cadena.
- **Tipo de artefacto**: Dependiendo de su selección, puede publicar los registros en Azure DevOps Server o en un recurso compartido al que pueda acceder el agente de compilación.
- **Herramientas**: puede optar por conservar los registros de herramientas específicas, o bien puede seleccionar **Todas las herramientas** para conservar todos los registros.

Para obtener información sobre la configuración de YAML para esta tarea, consulte nuestras [opciones de publicación de registros de seguridad de YAML](yaml-configuration.md#publish-security-analysis-logs-task).

## <a name="security-report-task"></a>Tarea de Security Report

En la captura de pantalla y la lista siguientes se muestran detalles de la configuración de Security Report.

![Configuración de la tarea de compilación de Security Report](./media/security-tools/4-createsecurityanalysisreport600.png)

- **Informes**: seleccione cualquiera de los siguientes formatos: **consola de la canalización**, **archivo TSV** y **archivo HTML**. Se crea un archivo de informe para cada formato seleccionado.
- **Herramientas**: seleccione las herramientas de la definición de compilación para las que quiere obtener un resumen de los problemas detectados. Para cada herramienta seleccionada, puede haber una opción para seleccionar si en el informe de resumen ve solo los errores o los errores y las advertencias.
- **Opciones avanzadas**: si no hay registros para alguna de las herramientas seleccionadas, puede optar por registrar una advertencia o un error. Si registra un error, se produce un error en la tarea.
- **Carpeta de registros base**: puede personalizar la carpeta de registros base donde se encuentran los registros. Sin embargo, esta opción no se utiliza normalmente.

Para obtener información sobre la configuración de YAML para esta tarea, consulte nuestras [opciones de informes de seguridad de YAML](yaml-configuration.md#security-report-task).

## <a name="post-analysis-task"></a>Tarea de Post-Analysis

En la captura de pantalla y la lista siguientes se muestran detalles de la configuración de tareas.

![Configuración de la tarea de compilación de Post-Analysis](./media/security-tools/a-post-analysis600.png)

- **Herramientas**: seleccione las herramientas de la definición de compilación para las que quiere insertar una interrupción de la compilación de forma condicional. Para cada herramienta seleccionada, puede haber una opción para seleccionar si quiere realizar una interrupción solo cuando haya errores o cuando haya errores y advertencias.
- **Informe**: puede escribir de forma opcional los resultados que provocan la interrupción de la compilación. Los resultados se escriben en el archivo de registro y la ventana de consola de Azure DevOps.
- **Opciones avanzadas**: si no hay registros para alguna de las herramientas seleccionadas, puede optar por registrar una advertencia o un error. Si registra un error, se produce un error en la tarea.

Para obtener información sobre la configuración de YAML para esta tarea, consulte nuestras [opciones de análisis posteriores de YAML](yaml-configuration.md#post-analysis-task).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre la configuración basada en YAML, consulte la [guía de configuración de YAML](yaml-configuration.md).

Si tiene más preguntas sobre la extensión de Análisis de código de seguridad y las herramientas que se ofrecen, consulte [nuestra página de preguntas más frecuentes](security-code-analysis-faq.md).

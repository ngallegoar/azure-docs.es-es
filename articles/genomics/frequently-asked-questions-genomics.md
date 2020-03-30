---
title: Preguntas frecuentes (P+F)
titleSuffix: Microsoft Genomics
description: Obtenga respuestas a preguntas comunes relacionadas con el uso del servicio de Microsoft Genomics, como información técnica, contrato de nivel de servicio y facturación.
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: troubleshooting
ms.date: 12/07/2017
ms.openlocfilehash: e8806bc4f761214e6740a22093b7e18030fdf881
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76986043"
---
# <a name="microsoft-genomics-common-questions"></a>Microsoft Genomics: preguntas frecuentes

En este artículo se enumeran las principales consultas que pueden surgir en relación con Microsoft Genomics. Para más información acerca del servicio de Microsoft Genomics, consulte [¿Qué es Microsoft Genomics?](overview-what-is-genomics.md). Para más información acerca de cómo solucionar problemas, consulte el documento [Troubleshooting guide](troubleshooting-guide-genomics.md) (Guía para la solución de problemas). 


## <a name="how-do-i-run-gatk4-workflows-on-microsoft-genomics"></a>¿Cómo se pueden ejecutar los flujos de trabajo de GATK4 en Microsoft Genomics?
En el archivo config.txt del servicio Microsoft Genomics, especifique el argumento process_name en `gatk4`. Tenga en cuenta que se le cobrarán tarifas normales de facturación.

## <a name="how-do-i-enable-output-compression"></a>¿Cómo habilito la compresión de salida?
Puede comprimir el archivo vcf o gvcf de salida con un argumento opcional para la compresión de salida. Esto es equivalente a ejecutar `-bgzip` seguido de `-tabix` en la salida del archivo vcf o gvcf para generar archivos `.gz` (salida de bgzip) y `.tbi` (salida de tabix). `bgzip` comprime el archivo vcf o gvcf y `tabix` crea un índice para el archivo comprimido. El argumento es un valor booleano, que se establece en `false` de forma predeterminada para la salida de vcf y `true` de forma predeterminada para la salida de gcvf. Para usarlo en la línea de comandos, especifique `-bz` o `--bgzip-output` como `true` (ejecute bgzip y tabix) o `false`. Para usar este argumento en el archivo config.txt, agregue `bgzip_output: true` o `bgzip_output: false` al archivo.

## <a name="what-is-the-sla-for-microsoft-genomics"></a>¿Qué es el SLA de Microsoft Genomics?
Le garantizamos que el 99,9 % de las veces el servicio de Microsoft Genomics estará disponible para recibir las solicitudes de API del flujo de trabajo. Para más información, consulte [Acuerdo de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/genomics/v1_0/).

## <a name="how-does-the-usage-of-microsoft-genomics-show-up-on-my-bill"></a>¿Cómo se refleja el uso de Microsoft Genomics en la factura?
Microsoft Genomics factura según el número de gigabases procesadas por flujo de trabajo. Para obtener más información, consulte el apartado [Precios](https://azure.microsoft.com/pricing/details/genomics/).


## <a name="where-can-i-find-a-list-of-all-possible-commands-and-arguments-for-the-msgen-client"></a>¿Dónde puedo encontrar una lista de todos los comandos y argumentos posibles para el cliente `msgen`?
Puede obtener una lista completa de comandos y argumentos disponibles ejecutando `msgen help`. Si no se proporciona ningún argumento más, se muestra una lista con las secciones de ayuda disponibles; una para cada uno de los comandos `submit`, `list`, `cancel` y `status`. Para obtener ayuda sobre un comando específico, escriba `msgen help command`; por ejemplo, `msgen help submit` enumera todas las opciones de envío.

## <a name="what-are-the-most-commonly-used-commands-for-the-msgen-client"></a>¿Cuáles son los comandos qué más usa el cliente `msgen`?
Los comandos más usados son argumentos del cliente `msgen` e incluyen: 

 |**Comando**          |  **Descripción del campo** |
 |:--------------------|:-------------         |
 |`list`               |Devuelve una lista con los trabajos que se han enviado. Para ver los argumentos, consulte `msgen help list`.  |
 |`submit`             |Envía una solicitud de flujo de trabajo al servicio. Para ver los argumentos, consulte `msgen help submit`.|
 |`status`             |Devuelve el estado del flujo de trabajo que especificó `--workflow-id`. Consulte también `msgen help status`. |
 |`cancel`             |Envía una solicitud para cancelar el procesamiento del flujo de trabajo que especificó `--workflow-id`. Consulte también `msgen help cancel`. |

## <a name="where-do-i-get-the-value-for---api-url-base"></a>¿Dónde se puede obtener el valor de `--api-url-base`?
Vaya a Azure Portal y abra la página de la cuenta de Genomics. En el encabezado **Administración**, elija **Claves de acceso**. Allí encontrará la URL de la API y las claves de acceso.

## <a name="where-do-i-get-the-value-for---access-key"></a>¿Dónde se puede obtener el valor de `--access-key`?
Vaya a Azure Portal y abra la página de la cuenta de Genomics. En el encabezado **Administración**, elija **Claves de acceso**. Allí encontrará la URL de la API y las claves de acceso.

## <a name="why-do-i-need-two-access-keys"></a>¿Por qué necesito dos claves de acceso?
Necesita dos claves de acceso en caso de que quiera actualizarlas (regenerarlas) sin interrumpir el uso del servicio. Por ejemplo, si quiere actualizar la primera clave, debe hacer que todos los flujos de trabajo nuevos usen la segunda clave. A continuación, espere a que todos los flujos de trabajo que usan la primera clave finalicen para poder actualizar dicha clave.

## <a name="do-you-save-my-storage-account-keys"></a>¿Se guardan mis claves de cuenta de almacenamiento?
La clave de cuenta de almacenamiento se usa para crear tokens de acceso a corto plazo para el servicio de Microsoft Genomics y así poder leer los archivos de entrada y escribir en los archivos de salida. La duración predeterminada del token es de 48 horas. La duración del token se puede cambiar con la opción `-sas/--sas-duration` del comando Enviar; el valor se indica en horas.

## <a name="what-genome-references-can-i-use"></a>¿Qué referencias genómicas puedo usar?

Se admiten estas referencias:

 |Referencia              | Valor de `-pa/--process-args` |
 |:-------------         |:-------------                 |
 |b37                    | `R=b37m1`                     |
 |hg38                   | `R=hg38m1`                    |      
 |hg38 (ningún análisis alternativo) | `R=hg38m1x`                   |  
 |hg19                   | `R=hg19m1`                    |    

## <a name="how-do-i-format-my-command-line-arguments-as-a-config-file"></a>¿Cómo puedo dar el formato de un archivo de configuración a los argumentos de la línea de comandos? 

msgen comprende los archivos de configuración en el formato siguiente:
* Todas las opciones se proporcionan como pares clave-valor con valores separados de las claves mediante dos puntos.
  Se ignora el espacio en blanco.
* Las líneas que empiezan con `#` se ignoran.
* Cualquier argumento de la línea de comandos que tenga un formato largo se puede convertir en una clave; para ello, hay que quitar los guiones principales y reemplazar los que están entre las palabras con guiones bajos. Estos son algunos ejemplos de conversión:

  |Argumento de línea de comandos            | Línea del archivo de configuración |
  |:-------------                   |:-------------                 |
  |`-u/--api-url-base https://url`  | *api_url_base: https://url*    |
  |`-k/--access-key KEY`            | *access_key:KEY*              |      
  |`-pa/--process-args R=B37m1`     | *process_args:R-b37m1*        |  

## <a name="next-steps"></a>Pasos siguientes

Use los siguientes recursos para empezar a trabajar con Microsoft Genomics:
- Ejecutar el primer flujo de trabajo mediante el servicio de Microsoft Genomics. [Ejecución de un flujo de trabajo mediante el servicio Microsoft Genomics](quickstart-run-genomics-workflow-portal.md)
- Enviar sus propios datos para que el servicio de Microsoft Genomics los procese: [FASTQ emparejados](quickstart-input-pair-FASTQ.md) | [BAM](quickstart-input-BAM.md) | [varios FASTQ o BAM](quickstart-input-multiple.md) 


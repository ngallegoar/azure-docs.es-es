---
title: Herramienta de firmware de Azure Kinect
description: Obtenga información sobre cómo consultar y actualizar el firmware del dispositivo mediante la herramienta de firmware de Azure Kinect.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: kinect, firmware, actualización
ms.openlocfilehash: f8b89751d27fb5a4b18d635f45f63f4f36bd05f6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85276973"
---
# <a name="azure-kinect-dk-firmware-tool"></a>Herramienta de irmware de Azure Kinect DK

La herramienta de firmware de Azure Kinect se puede usar para consultar y actualizar el firmware del dispositivo de Azure Kinect DK.

## <a name="list-connected-devices"></a>Enumeración de los dispositivos conectados

Puede obtener una lista de los dispositivos conectados mediante la opción -l.  `AzureKinectFirmwareTool.exe -l`

```console
 == Azure Kinect DK Firmware Tool ==
Found 2 connected devices:
0: Device "000036590812"
1: Device "000274185112"
```

## <a name="check-device-firmware-version"></a>Comprobación de la versión de firmware del dispositivo

Puede comprobar las versiones de firmware actuales del primer dispositivo asociado mediante la opción -q; por ejemplo, `AzureKinectFirmwareTool.exe -q`.

```console
 == Azure Kinect DK Firmware Tool ==
Device Serial Number: 000036590812
Current Firmware Versions:
    RGB camera firmware:      1.5.92
    Depth camera firmware:    1.5.66
    Depth config file:        6109.7
    Audio firmware:           1.5.14
    Build Config:             Production
    Certificate Type:         Microsoft
```

Si hay más de un dispositivo asociado, puede especificar el dispositivo que quiere consultar al agregar el número de serie completo al comando, por ejemplo:

`AzureKinectFirmwareTool.exe -q 000036590812`

## <a name="update-device-firmware"></a>Actualización del firmware del dispositivo

El uso más común de esta herramienta consiste en actualizar el firmware del dispositivo. Realice la actualización mediante una llamada a la herramienta con la opción `-u`. Una actualización de firmware puede tardar unos minutos, en función de los archivos de firmware que se deban actualizar.

Para obtener instrucciones paso a paso de actualización del firmware, consulte [Actualización de firmware de Azure Kinect](update-device-firmware.md).  

`AzureKinectFirmwareTool.exe -u firmware\AzureKinectDK_Fw_1.5.926614.bin`

Si hay más de un dispositivo asociado, puede especificar el dispositivo que quiere consultar al agregar el número de serie completo al comando.

`AzureKinectFirmwareTool.exe -u firmware\AzureKinectDK_Fw_1.5.926614.bin 000036590812`

## <a name="reset-device"></a>Restablecer el dispositivo

Se puede restablecer una instancia de Azure Kinect DK asociada mediante la opción -r si debe establecer el dispositivo en un estado conocido.

Si hay más de un dispositivo asociado, puede especificar el dispositivo que quiere consultar al agregar el número de serie completo al comando.

`AzureKinectFirmwareTool.exe -r 000036590812`

## <a name="inspect-firmware"></a>Inspección del firmware

La inspección del firmware permite obtener la información de versión de un archivo .bin de firmware antes de actualizar un dispositivo real.

`AzureKinectFirmwareTool.exe -i firmware\AzureKinectDK_Fw_1.5.926614.bin`

```console
 == Azure Kinect DK Firmware Tool ==
Loading firmware package ..\tools\updater\firmware\AzureKinectDK_Fw_1.5.926614.bin.
File size: 1228844 bytes
This package contains:
  RGB camera firmware:      1.5.92
  Depth camera firmware:    1.5.66
  Depth config files: 6109.7 5006.27
  Audio firmware:           1.5.14
  Build Config:             Production
  Certificate Type:         Microsoft
  Signature Type:           Microsoft
```

## <a name="firmware-update-tool-options"></a>Opciones de la herramienta de actualización de firmware

```console
 == Azure Kinect DK Firmware Tool ==
* Usage Info *
    AzureKinectFirmwareTool.exe <Command> <Arguments>

Commands:
    List Devices: -List, -l
    Query Device: -Query, -q
        Arguments: [Serial Number]
    Update Device: -Update, -u
        Arguments: <Firmware Package Path and FileName> [Serial Number]
    Reset Device: -Reset, -r
        Arguments: [Serial Number]
    Inspect Firmware: -Inspect, -i
        Arguments: <Firmware Package Path and FileName>

    If no Serial Number is provided, the tool will just connect to the first device.

Examples:
    AzureKinectFirmwareTool.exe -List
    AzureKinectFirmwareTool.exe -Update c:\data\firmware.bin 0123456
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[Instrucciones paso a paso para actualizar el firmware del dispositivo](update-device-firmware.md)

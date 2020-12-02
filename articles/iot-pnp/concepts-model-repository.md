---
title: Descripción de los conceptos del repositorio de modelos de dispositivo | Microsoft Docs
description: La finalidad de este artículo es que tanto los profesionales de TI como los desarrolladores de soluciones conozcan los conceptos básicos del repositorio de modelos de dispositivo.
author: rido-min
ms.author: rmpablos
ms.date: 11/17/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: b567efe2541bb33c905def73bb78398799b4ed69
ms.sourcegitcommit: 03c0a713f602e671b278f5a6101c54c75d87658d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2020
ms.locfileid: "94920549"
---
# <a name="device-model-repository"></a>Repositorio de modelos de dispositivo

El repositorio de modelos de dispositivo (DMR) permite a los creadores de dispositivos administrar y compartir los modelos de dispositivos IoT Plug and Play. Los modelos de dispositivo son documentos LD JSON definidos mediante el [lenguaje de creación de modelos de Digital Twins (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md).

DMR define un patrón para almacenar interfaces de DTDL en una estructura de carpetas basada en el identificador de modelo de dispositivos gemelos (DTMI). Para buscar una interfaz en DMR, puede convertir el DTMI en una ruta de acceso relativa. Por ejemplo, el DTMI `dtmi:com:example:Thermostat;1` se traduce en `/dtmi/com/example/thermostat-1.json`.

## <a name="public-device-model-repository"></a>Repositorio de modelos de dispositivo públicos

Microsoft hospeda un DMR público con estas características:

- Modelos seleccionados. Microsoft revisa y aprueba todas las interfaces disponibles mediante un flujo de trabajo de validación de la solicitud de incorporación de cambios de GitHub (PR).
- Inmutabilidad.  Una vez publicada, una interfaz no se puede actualizar.
- Hiperescala Microsoft proporciona la infraestructura necesaria para crear un punto de conexión seguro y escalable en el que pueda publicar y consumir modelos de dispositivos.

## <a name="custom-device-model-repository"></a>Repositorio de modelos de dispositivo personalizados

Use el mismo patrón de DMR para crear un DMR personalizado en cualquier medio de almacenamiento, como el sistema de archivos local o servidores web HTTP personalizados. Los modelos de dispositivos se pueden recuperar del DMR personalizado de la misma manera que desde el DMR público, cambiando la dirección URL base que se usa para acceder al DMR.

> [!NOTE]
> Microsoft proporciona las herramientas necesarias para validar modelos de dispositivos en el DMR público. Estas herramientas se pueden usar también en repositorios personalizados.

## <a name="public-models"></a>Modelos públicos

Los modelos de dispositivos públicos almacenados en el repositorio de modelos están disponibles para que todos los usuarios los consuman e integren en sus aplicaciones. Los modelos de dispositivos públicos permiten un ecosistema abierto donde creadores de dispositivos y desarrolladores de soluciones puedan compartir y reutilizar sus modelos de dispositivo IoT Plug and Play.

Consulte la sección [Publicación de modelos](#publish-a-model) para obtener instrucciones sobre cómo publicar un modelo en el repositorio de modelos para que sea público.

Los usuarios pueden examinar, buscar y ver las interfaces públicas desde el [repositorio de GitHub](https://github.com/Azure/iot-plugandplay-models) oficial.

Todas las interfaces de las carpetas `dtmi` también están disponibles en el punto de conexión público.[https://devicemodels.azure.com](https://devicemodels.azure.com)

### <a name="resolve-models"></a>Resolver modelos

Para acceder a estas interfaces mediante programación, es preciso convertir un DTMI en una ruta de acceso relativa que se pueda usar para consultar el punto de conexión público.

Para convertir un DTMI en una ruta de acceso absoluta, use la función `DtmiToPath` con `IsValidDtmi`:

```cs
static string DtmiToPath(string dtmi)
{
    if (!IsValidDtmi(dtmi))
    {
        return null;
    }
    // dtmi:com:example:Thermostat;1 -> dtmi/com/example/thermostat-1.json
    return $"/{dtmi.ToLowerInvariant().Replace(":", "/").Replace(";", "-")}.json";
}

static bool IsValidDtmi(string dtmi)
{
    // Regex defined at https://github.com/Azure/digital-twin-model-identifier#validation-regular-expressions
    Regex rx = new Regex(@"^dtmi:[A-Za-z](?:[A-Za-z0-9_]*[A-Za-z0-9])?(?::[A-Za-z](?:[A-Za-z0-9_]*[A-Za-z0-9])?)*;[1-9][0-9]{0,8}$");
    return rx.IsMatch(dtmi);
}
```

Con la ruta de acceso resultante y la dirección URL base para el repositorio, podemos obtener la interfaz:

```cs
const string _repositoryEndpoint = "https://devicemodels.azure.com";

string dtmiPath = DtmiToPath(dtmi.ToString());
string fullyQualifiedPath = $"{_repositoryEndpoint}{dtmiPath}";
string modelContent = await _httpClient.GetStringAsync(fullyQualifiedPath);
```

## <a name="publish-a-model"></a>Publicación de modelos

> [!Important]
> Debe tener una cuenta de GitHub para poder enviar modelos al DMR público.

1. Bifurque el repositorio de GitHub público: [https://github.com/Azure/iot-plugandplay-models](https://github.com/Azure/iot-plugandplay-models).
1. Clone el repositorio bifurcado. Opcionalmente, puede crear una nueva rama para mantener los cambios aislados de la rama `main`.
1. Agregue las nuevas interfaces a la carpeta `dtmi` con la convención carpeta/nombre de archivo. Para obtener más información, consulte [Importación de un modelo en la carpeta `dtmi/`](#import-a-model-to-the-dtmi-folder).
1. Valide los modelos localmente mediante la herramienta `dmr-client`. Para obtener más información, vea [Validación de modelos](#validate-models).
1. Confirme los cambios de forma local y envíelos a la bifurcación.
1. Desde la bifurcación, cree una solicitud de incorporación de cambios que tenga como destino la rama `main`. Consulte la documentación para [Crear una incidencia o una solicitud de incorporación de cambios](https://docs.github.com/free-pro-team@latest/desktop/contributing-and-collaborating-using-github-desktop/creating-an-issue-or-pull-request).
1. Revise los [requisitos de solicitud de incorporación de cambios](https://github.com/Azure/iot-plugandplay-models/blob/main/pr-reqs.md).

La solicitud de incorporación de cambios desencadena una serie de acciones de GitHub que validan las interfaces enviadas y se asegurará de que dicha solicitud supera todos los requisitos.

Microsoft responderá a las solicitudes de incorporación de cambios con todas las comprobaciones en tres días laborables.

### <a name="dmr-client-tools"></a>Herramientas `dmr-client`

Las herramientas que se usan para validar los modelos durante las comprobaciones de solicitud de incorporación de cambios también se pueden usar para agregar y validar las interfaces DTDL localmente.

> [!NOTE]
> Esta herramienta requiere la versión 3.1 o superior del [SDK de .NET](https://dotnet.microsoft.com/download).

### <a name="install-dmr-client"></a>Instalar `dmr-client`

```bash
curl -L https://aka.ms/install-dmr-client-linux | bash
```

```powershell
iwr https://aka.ms/install-dmr-client-windows -UseBasicParsing | iex
```

### <a name="import-a-model-to-the-dtmi-folder"></a>Importación de un modelo a la carpeta `dtmi/`

Si el modelo ya está almacenado en archivos .json, puede usar el comando `dmr-client import` para agregarlos a la carpeta `dtmi/` con los nombres de archivo correctos:

```bash
# from the local repo root folder
dmr-client import --model-file "MyThermostat.json"
```

> [!TIP]
> Puede usar el argumento `--local-repo` para especificar la carpeta raíz del repositorio local.

### <a name="validate-models"></a>Validación de modelos

Puede validar los modelos con el comando `dmr-client validate`:

```bash
dmr-client validate --model-file ./my/model/file.json
```

> [!NOTE]
> La validación utiliza la versión del analizador de DTDL más reciente para asegurarse de que todas las interfaces son compatibles con la especificación del lenguaje DTDL.

Para validar las dependencias externas, deben existir en el repositorio local. Para validar los modelos, use la opción `--repo` para especificar una carpeta `local` o `remote` para resolver las dependencias:

```bash
# from the repo root folder
dmr-client validate --model-file ./my/model/file.json --repo .
```

### <a name="strict-validation"></a>Validación estricta

El DMR incluye requisitos [adicionales](https://github.com/Azure/iot-plugandplay-models/blob/main/pr-reqs.md), use la marca `stict` para validar el modelo respecto a estos:

```bash
dmr-client validate --model-file ./my/model/file.json --repo . --strict true
```

Compruebe si en la salida de la consola hay algún mensaje de error.

### <a name="export-models"></a>Exportación de modelos

Los modelos se pueden exportar desde un repositorio determinado (local o remoto) a un solo archivo mediante una matriz JSON:

```bash
dmr-client export --dtmi "dtmi:com:example:TemperatureController;1" -o TemperatureController.expanded.json
```

## <a name="next-steps"></a>Pasos siguientes

El paso siguiente sugerido es revisar la [arquitectura de IoT Plug and Play](concepts-architecture.md).

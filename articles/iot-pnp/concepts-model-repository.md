---
title: Descripción de los conceptos del repositorio de modelos de dispositivo | Microsoft Docs
description: La finalidad de este artículo es que los profesionales de TI o los desarrolladores de soluciones conozcan los conceptos básicos del repositorio de modelos de dispositivo.
author: rido-min
ms.author: rmpablos
ms.date: 09/30/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: cfdf22ac9b97ff7187bc360efe07cfe16249bd6b
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92042888"
---
# <a name="device-model-repository"></a>Repositorio de modelos de dispositivo

El repositorio de modelos de dispositivo (DMR) permite a los creadores de dispositivos administrar y compartir los modelos de dispositivos IoT Plug and Play. Los modelos de dispositivo son documentos LD JSON definidos mediante el [lenguaje de creación de modelos de Digital Twins (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md).

DMR define un patrón para almacenar interfaces de DTDL en una estructura de carpetas basada en el identificador de modelo de dispositivos gemelos (DTMI). Para buscar una interfaz en DMR, puede convertir el DTMI en una ruta de acceso relativa. Por ejemplo, el DTMI `dtmi:com:example:Thermostat;1` se traduce en `/dtmi/com/example/thermostat-1.json`.

## <a name="public-device-model-repository"></a>Repositorio de modelos de dispositivo públicos

Microsoft hospeda un DMR público con estas características:

- Modelos seleccionados. Microsoft revisa y aprueba todas las interfaces disponibles mediante un flujo de trabajo de validación de PR de GitHub abierto.
- Inmutabilidad.  Una vez publicada, una interfaz no se puede actualizar.
- Hiperescala Microsoft proporciona toda la infraestructura necesaria para crear un punto de conexión seguro y altamente escalable.

## <a name="custom-device-model-repository"></a>Repositorio de modelos de dispositivo personalizados

Puede usar el mismo patrón de DMR en cualquier medio de almacenamiento, como el sistema de archivos local o servidores web HTTP personalizados, para crear un DMR personalizado. Puede recuperar modelos del DMR personalizados de la misma manera que desde el DMR público. Solo tiene que cambiar la dirección URL base que se usa para acceder al DMR.

> [!NOTE]
> Las herramientas que se usan para validar los modelos en el DMR público se pueden reutilizar en repositorios personalizados.

## <a name="public-models"></a>Modelos públicos

Los modelos de gemelos digitales públicos almacenados en el repositorio de modelos están disponibles para que todos los usuarios los consuman e integren en sus aplicaciones. Los modelos públicos permiten un ecosistema abierto donde creadores de dispositivos y desarrolladores de soluciones puedan compartir y reutilizar sus modelos de dispositivo IoT Plug and Play.

Consulte la sección [Publicación de modelos](#publish-a-model) para obtener instrucciones sobre cómo publicar un modelo en el repositorio de modelos para que sea público.

Los usuarios pueden examinar, buscar y ver las interfaces públicas desde el [repositorio de GitHub](https://github.com/Azure/iot-plugandplay-models) oficial.

Todas las interfaces de las carpetas `dtmi` también están disponibles en el punto de conexión público.[https://devicemodels.azure.com](https://devicemodels.azure.com)

### <a name="resolve-models"></a>Resolver modelos

Para acceder mediante programación a estas interfaces, debe convertir un DTMI en una ruta de acceso relativa que pueda usar para consultar el punto de conexión público. En el ejemplo de código siguiente se muestra cómo hacerlo:

Para convertir un DTMI en una ruta de acceso absoluta, usamos la función `DtmiToPath` con `IsValidDtmi`:

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
1. Agregue las nuevas interfaces a la carpeta `dtmi` con la convención carpeta/nombre de archivo. Consulte la herramienta [add-model](#add-model).
1. Valide los modelos localmente mediante la sección [scripts para validar cambios](#validate-files).
1. Confirme los cambios de forma local y envíelos a la bifurcación.
1. Desde la bifurcación, cree una PR que tenga como destino la rama `main`. Consulte la documentación para [Crear una incidencia o una solicitud de incorporación de cambios](https://docs.github.com/free-pro-team@latest/desktop/contributing-and-collaborating-using-github-desktop/creating-an-issue-or-pull-request).
1. Revise los [Requisitos de PR](https://github.com/Azure/iot-plugandplay-models/blob/main/pr-reqs.md).

La PR desencadena una serie de acciones de GitHub que validarán las nuevas interfaces enviadas y se asegurará de que la PR supera todas las comprobaciones.

Microsoft responderá a una PR con todas las comprobaciones en tres días laborables.

### <a name="add-model"></a>add-model

En los pasos siguientes se muestra cómo el script add-model.js le ayuda a agregar una nueva interfaz. Este script requiere Node.js para ejecutarse:

1. En un símbolo del sistema, navegue al repositorio de GIT local.
1. Ejecute `npm install`:
1. Ejecute `npm run add-model <path-to-file-to-add>`:

Observe si en la salida de la consola hay algún mensaje de error.

### <a name="local-validation"></a>Validación local

Puede ejecutar las mismas comprobaciones de validación localmente antes de enviar la PR para ayudar a diagnosticar problemas de antemano.

#### <a name="validate-files"></a>validate-files

`npm run validate-files <file1.json> <file2.json>` comprueba que la ruta de acceso del archivo coincide con los nombres de archivo y carpeta esperados.

#### <a name="validate-ids"></a>validate-ids

`npm run validate-ids <file1.json> <file2.json>` comprueba que todos los identificadores definidos en el documento usan la misma raíz que el identificador principal.

#### <a name="validate-deps"></a>validate-deps

`npm run validate-deps <file1.json> <file2.json>` comprueba que todas las dependencias están disponibles en la carpeta `dtmi`.

#### <a name="validate-models"></a>validate-models

Puede ejecutar la [muestra de validación de DTDL](https://github.com/Azure-Samples/DTDL-Validator) para validar los modelos localmente.

## <a name="next-steps"></a>Pasos siguientes

El paso siguiente sugerido es revisar la [arquitectura de IoT Plug and Play](concepts-architecture.md).
---
title: Configuración de la conversión de modelos
description: Descripción de todos los parámetros de conversión de modelos
author: florianborn71
ms.author: flborn
ms.date: 03/06/2020
ms.topic: how-to
ms.openlocfilehash: 83f80f893620a225c928be2ad7ad1679b3a9c465
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83652237"
---
# <a name="configure-the-model-conversion"></a>Configuración de la conversión de modelos

En este capítulo se documentan las opciones para la conversión de modelos.

## <a name="settings-file"></a>Archivo de configuración

Si se encuentra un archivo denominado `ConversionSettings.json` en el contenedor de entrada junto al modelo de entrada, se usará para proporcionar una configuración adicional para el proceso de conversión de modelos.

El contenido del archivo debe cumplir el esquema JSON siguiente:

```json
{
    "$schema" : "http://json-schema.org/schema#",
    "description" : "ARR ConversionSettings Schema",
    "type" : "object",
    "properties" :
    {
        "scaling" : { "type" : "number", "exclusiveMinimum" : 0, "default" : 1.0 },
        "recenterToOrigin" : { "type" : "boolean", "default" : false },
        "opaqueMaterialDefaultSidedness" : { "type" : "string", "enum" : [ "SingleSided", "DoubleSided" ], "default" : "DoubleSided" },
        "material-override" : { "type" : "string", "default" : "" },
        "gammaToLinearMaterial" : { "type" : "boolean", "default" : false },
        "gammaToLinearVertex" : { "type" : "boolean", "default" : false },
        "sceneGraphMode": { "type" : "string", "enum" : [ "none", "static", "dynamic" ], "default" : "dynamic" },
        "generateCollisionMesh" : { "type" : "boolean", "default" : true },
        "unlitMaterials" : { "type" : "boolean", "default" : false },
        "fbxAssumeMetallic" : { "type" : "boolean", "default" : true },
        "deduplicateMaterials" : { "type" : "boolean", "default" : true },
        "axis" : {
            "type" : "array",
            "items" : {
                "type" : "string",
                "enum" : ["default", "+x", "-x", "+y", "-y", "+z", "-z"]
            },
            "minItems": 3,
            "maxItems": 3
        }
    },
    "additionalProperties" : false
}
```

Un archivo `ConversionSettings.json` de ejemplo podría ser:

```json
{
    "scaling" : 0.01,
    "recenterToOrigin" : true,
    "material-override" : "box_materials_override.json"
}
```

### <a name="geometry-parameters"></a>Parámetros de geometría

* `scaling`: este parámetro escala uniformemente un modelo. El escalado se puede usar para aumentar o reducir un modelo, por ejemplo, para mostrar un modelo de compilación en la parte superior de una tabla. Dado que el motor de representación espera que las longitudes se especifiquen en metros, surge otro uso importante de este parámetro cuando un modelo se define en unidades distintas. Por ejemplo, si un modelo se define en centímetros, la aplicación de una escala de 0,01 debe representar el modelo con el tamaño correcto.
Algunos formatos de datos de origen (por ejemplo, .fbx) proporcionan una sugerencia de escalado de unidades, en cuyo caso la conversión escala implícitamente el modelo a unidades de medidor. El escalado implícito proporcionado por el formato de origen se aplicará en la parte superior del parámetro de escalado.
El factor de escalado final se aplica a los vértices de geometría y las transformaciones locales de los nodos de gráfico de escena. El escalado para la transformación de la entidad raíz permanece sin modificar.

* `recenterToOrigin`: indica que un modelo debe convertirse para que su rectángulo de selección se centre en el origen.
Centrar es importante si el modelo de origen se desplaza lejos del origen, ya que, en ese caso, las incidencias de precisión del número de punto flotante pueden dar lugar a artefactos de representación.

* `opaqueMaterialDefaultSidedness`: el motor de representación da por sentado que los materiales opacos son de doble cara.
Si no es el comportamiento previsto, este parámetro debe establecerse en "SingleSided". Para más información, consulte [Representación de un solo lado](../../overview/features/single-sided-rendering.md).

### <a name="material-overrides"></a>Invalidaciones de materiales

* `material-override`: este parámetro permite que el procesamiento de materiales se [personalice durante la conversión](override-materials.md).

### <a name="material-de-duplication"></a>Desduplicación de materiales

* `deduplicateMaterials`: este parámetro habilita o deshabilita la desduplicación automática de materiales que comparten las mismas propiedades y texturas. La desduplicación se produce una vez procesados los reemplazos de material. Esto está habilitada de manera predeterminada.

### <a name="color-space-parameters"></a>Parámetros de espacio de colores

El motor de representación espera que los valores del color estén en el espacio lineal.
Si un modelo se define mediante el espacio gamma, estas opciones deben establecerse en true.

* `gammaToLinearMaterial`: conversión de colores del material del espacio gamma al espacio lineal
* `gammaToLinearVertex`: conversión de colores del vértice del espacio gamma al espacio lineal

> [!NOTE]
> En el caso de los archivos FBX, esta configuración se establece en `true` de forma predeterminada. En el caso del resto de los tipos de archivo, el valor predeterminado es `false`.

### <a name="scene-parameters"></a>Parámetros de escena

* `sceneGraphMode`: define cómo se convierte el gráfico de escena en el archivo de origen:
  * `dynamic` (predeterminado): todos los objetos del archivo se exponen como [entidades](../../concepts/entities.md) en la API y se pueden transformar de manera independiente. La jerarquía de nodos en tiempo de ejecución es idéntica a la estructura del archivo de origen.
  * `static`: todos los objetos se exponen en la API, pero no se pueden transformar de manera independiente.
  * `none`: el gráfico de escena se contrae en un objeto.

Cada modo tiene un rendimiento en tiempo de ejecución diferente. En el modo `dynamic`, el costo de rendimiento se escala linealmente con el número de [entidades](../../concepts/entities.md) del gráfico, incluso cuando no se mueve ninguna parte. Solo se debe usar cuando es necesario mover partes individualmente para la aplicación, por ejemplo, para una animación de "vista de explosión".

El modo de `static` exporta el gráfico de escena completo, pero las partes que contiene este gráfico tienen una transformación constante relativa a su parte raíz. El nodo raíz del objeto, sin embargo, puede seguir moviéndose, girándose o escalándose sin ningún costo de rendimiento importante. Además, las [consultas espaciales](../../overview/features/spatial-queries.md) devolverán partes individuales, pudiendo modificarse cada una a través de [invalidaciones de estado](../../overview/features/override-hierarchical-state.md). Con este modo, la sobrecarga en tiempo de ejecución por objeto es insignificante. Es ideal para escenas grandes en las que aún es necesaria una inspección por objeto, pero no cambia ninguna transformación por objeto.

El modo `none` tiene la sobrecarga en tiempo de ejecución más baja, así como tiempos de carga ligeramente mejores. La inspección o transformación de objetos individuales no es posible en este modo. Los casos de uso son, por ejemplo, modelos de fotogrametría que no tienen un gráfico de escena significativo en primer lugar.

> [!TIP]
> Muchas aplicaciones cargarán varios modelos. Debe optimizar los parámetros de conversión de cada modelo en función de cómo se vaya a utilizar. Por ejemplo, si desea mostrar el modelo de un automóvil para que el usuario lo desmonte e inspeccione con detalle, deberá convertirlo con el modo `dynamic`. Sin embargo, si además desea colocar el automóvil en un entorno de sala de exposiciones, ese modelo puede convertirse con `sceneGraphMode` establecido en `static` o incluso `none`.

### <a name="physics-parameters"></a>Parámetros de física

* `generateCollisionMesh`: si necesita compatibilidad con [consultas espaciales](../../overview/features/spatial-queries.md) en un modelo, debe habilitarse esta opción. En el peor de los casos, la creación de una malla de colisión puede duplicar el tiempo de conversión. Los modelos con mallas de colisión tardan más tiempo en cargarse y, al usar un gráfico de escena `dynamic`, también tienen una mayor sobrecarga de rendimiento en tiempo de ejecución. Para obtener un rendimiento óptimo general, debe deshabilitar esta opción en todos los modelos en los que no son necesarias las consultas espaciales.

### <a name="unlit-materials"></a>Materiales sin iluminación

* `unlitMaterials`: de forma predeterminada, la conversión preferirá crear [materiales de PBR](../../overview/features/pbr-materials.md). Esta opción indica al convertidor que trate todos los materiales como [materiales de color](../../overview/features/color-materials.md) en su lugar. Si tiene datos que ya incorporan iluminación, como, por ejemplo, los modelos creados a través de fotogrametría, esta opción le permite aplicar rápidamente la conversión correcta para todos los materiales, sin necesidad de [invalidar cada material](override-materials.md) de forma individual.

### <a name="converting-from-older-fbx-formats-with-a-phong-material-model"></a>Conversión a partir de formatos de FBX anteriores, con un modelo de material de Phong

* `fbxAssumeMetallic`: las versiones anteriores del formato de FBX definen sus materiales mediante un modelo de material de Phong. El proceso de conversión debe deducir cómo se asignan estos materiales al [modelo de PBR](../../overview/features/pbr-materials.md) del representador. Normalmente esto funciona bien, pero puede surgir una ambigüedad cuando un material no tiene texturas, valores especulares altos ni colores albedo no grises. En este caso, la conversión debe decidir entre priorizar los valores especulares altos, definir un material metálico altamente reflectante en el que el color albedo se disuelve o priorizar el color albedo, lo que define algo similar a un plástico de colores brillantes. De forma predeterminada, en el proceso de conversión se da por sentado que los valores altamente especulares implican un material metálico en los casos en los que se aplica ambigüedad. Este parámetro se puede establecer en `false` para cambiar al contrario.

### <a name="coordinate-system-overriding"></a>Invalidación del sistema de coordenadas

* `axis`: para invalidar los vectores de unidad del sistema de coordenadas. Los valores predeterminados son `["+x", "+y", "+z"]`. En teoría, el formato de FBX tiene un encabezado en el que se definen esos vectores y la conversión usa esa información para transformar la escena. El formato glTF también define un sistema de coordenadas fijo. En la práctica, algunos recursos tienen información incorrecta en su encabezado o se han guardado con una convención diferente del sistema de coordenadas. Esta opción permite invalidar el sistema de coordenadas que se va a compensar. Por ejemplo: `"axis" : ["+x", "+z", "-y"]` intercambiará el eje Z y el eje Y y mantendrá la mano del sistema de coordenadas invirtiendo la dirección del eje Y.

### <a name="vertex-format"></a>Formato de vértice

Es posible ajustar el formato de vértice de una malla para cambiar la precisión del ahorro de memoria. Una superficie de memoria más baja permite cargar modelos más grandes o conseguir un mejor rendimiento. Sin embargo, en función de los datos, el formato incorrecto puede afectar significativamente a la calidad de la representación.

> [!CAUTION]
> Cambiar el formato de vértice debe ser el último recurso si los modelos ya no caben en la memoria o al optimizar el mejor rendimiento posible. Los cambios pueden presentar fácilmente artefactos de representación, ambos obvios y sutiles. A menos que sepa qué buscar, no debe cambiar el valor predeterminado.

Estos ajustes son posibles:

* Los flujos de datos específicos pueden incluirse o excluirse de forma explícita.
* La precisión de los flujos de datos puede reducirse para, a su vez, reducir la superficie de memoria.

La sección `vertex` siguiente del archivo `.json` es opcional. En el caso de cada una de las partes no especificadas explícitamente, el servicio de conversión recurre a su configuración predeterminada.

```json
{
    ...
    "vertex" : {
        "position"  : "32_32_32_FLOAT",
        "color0"    : "NONE",
        "color1"    : "NONE",
        "normal"    : "NONE",
        "tangent"   : "NONE",
        "binormal"  : "NONE",
        "texcoord0" : "32_32_FLOAT",
        "texcoord1" : "NONE"
    },
    ...
```

Al forzar a un componente a `NONE`, se garantiza que la malla de salida no tenga el flujo correspondiente.

#### <a name="component-formats-per-vertex-stream"></a>Formatos de componentes por flujo de vértices

Estos formatos están permitidos para los componentes correspondientes:

| Componente de vértice | Formatos admitidos (negrita = valor predeterminado) |
|:-----------------|:------------------|
|position| **32_32_32_FLOAT**, 16_16_16_16_FLOAT |
|color0| **8_8_8_8_UNSIGNED_NORMALIZED**, NONE |
|color1| 8_8_8_8_UNSIGNED_NORMALIZED, **NONE**|
|normal| **8_8_8_8_SIGNED_NORMALIZED**, 16_16_16_16_FLOAT, NONE |
|tangente| **8_8_8_8_SIGNED_NORMALIZED**, 16_16_16_16_FLOAT, NONE |
|binormal| **8_8_8_8_SIGNED_NORMALIZED**, 16_16_16_16_FLOAT, NONE |
|texcoord0| **32_32_FLOAT**, 16_16_FLOAT, NONE |
|texcoord1| **32_32_FLOAT**, 16_16_FLOAT, NONE |

#### <a name="supported-component-formats"></a>Formatos de componentes compatibles

Las superficies de memoria de los formatos son las siguientes:

| Formato | Descripción | Bytes por vértice |
|:-------|:------------|:---------------|
|32_32_FLOAT|precisión del número de punto flotante completa de dos componentes|8
|16_16_FLOAT|precisión del número de punto flotante media de dos componentes|4
|32_32_32_FLOAT|precisión del número de punto flotante completa de tres componentes|12
|16_16_16_16_FLOAT|precisión del número de punto flotante media de cuatro componentes|8
|8_8_8_8_UNSIGNED_NORMALIZED|byte de cuatro componentes, normalizado en el intervalo `[0; 1]`|4
|8_8_8_8_SIGNED_NORMALIZED|byte de cuatro componentes, normalizado en el intervalo `[-1; 1]`|4

#### <a name="best-practices-for-component-format-changes"></a>Procedimientos recomendados para los cambios del formato de componentes

* `position`: es poco habitual que la precisión reducida sea suficiente. **16_16_16_16_FLOAT** presenta artefactos de cuantificación perceptibles, incluso para modelos pequeños.
* `normal`, `tangent`, `binormal`: normalmente, estos valores se cambian juntos. A menos que haya artefactos de iluminación perceptibles derivados de la cuantificación normal, no hay ninguna razón para aumentar su precisión. En algunos casos, sin embargo, estos componentes se pueden establecer en **NONE**:
  * `normal`, `tangent` y `binormal` solo son necesarios si se debe iluminar al menos un material del modelo. En ARR, este es el caso al usarse un [material de PBR](../../overview/features/pbr-materials.md) en el modelo en cualquier momento.
  * `tangent` y `binormal` solo son necesarios si alguno de los materiales de iluminación usa una textura de mapa normal.
* `texcoord0`, `texcoord1`: Las coordenadas de textura pueden usar una precisión reducida (**16_16_FLOAT**) si sus valores permanecen en el intervalo `[0; 1]` y si las texturas tratadas tienen un tamaño máximo de 2048 x 2048 píxeles. Si se superan esos límites, la calidad de la asignación de textura se verá afectada.

#### <a name="example"></a>Ejemplo

Suponga que tiene un modelo de fotogrametría, cuya iluminación está preparada en las texturas. Todo lo que se necesita para representar el modelo son las posiciones de los vértices y las coordenadas de textura.

De forma predeterminada, el convertidor debe suponer que es posible que desee usar materiales de PBR en un modelo en algún momento, por lo que generará datos relativos a `normal`, `tangent` y `binormal` automáticamente. Por lo tanto, el uso de memoria por vértice es `position` (12 bytes) + `texcoord0` (8 bytes) + `normal` (4 bytes) + `tangent` (4 bytes) + `binormal` (4 byte) = 32 bytes. Los modelos más grandes de este tipo pueden tener fácilmente muchos millones de vértices que dan como resultado modelos que pueden aceptar varios gigabytes de memoria. Estas grandes cantidades de datos afectarán al rendimiento y es posible que incluso se quede sin memoria.

Al saber que nunca va a necesitar iluminación dinámica en el modelo y que todas las coordenadas de textura están en el intervalo `[0; 1]`, puede establecer `normal`, `tangent` y `binormal` en `NONE` y `texcoord0` en precisión media (`16_16_FLOAT`), lo que da como resultado solo 16 bytes por vértice. La reducción de los datos de malla a la mitad permite cargar modelos más grandes y mejora el rendimiento potencialmente.

## <a name="typical-use-cases"></a>Casos de uso típicos

Buscar una buena configuración de importación para un caso de uso determinado puede ser un proceso tedioso. Por otro lado, la configuración de conversión puede afectar de forma significativa al rendimiento en tiempo de ejecución.

Existen ciertas clases de casos de uso que son aptas para optimizaciones específicas. A continuación se proporcionan algunos ejemplos.

### <a name="use-case-architectural-visualization--large-outdoor-maps"></a>Caso de uso: Visualización de arquitectura/grandes mapas exteriores

* Estos tipos de escenas tienden a ser estáticos, lo que significa que no necesitan partes movibles. En consecuencia, el modo `sceneGraphMode` se puede establecer en `static` o incluso `none`, que mejora el rendimiento en tiempo de ejecución. Con el modo `static`, el nodo raíz de la escena puede seguir moviéndose, girándose y escalándose, por ejemplo, para cambiar de forma dinámica entre la escala de 1:1 (para la vista de primera persona) y una vista principal de tabla.

* Si necesita mover partes, normalmente también significa que necesita compatibilidad con proyecciones de rayo u otras [consultas espaciales](../../overview/features/spatial-queries.md), de modo que pueda elegir esas partes en primer lugar. Por otro lado, si no tiene previsto mover nada, lo más probable es que tampoco lo necesite para participar en consultas espaciales y, por lo tanto, pueda desactivar la marca `generateCollisionMesh`. Este modificador tiene un impacto significativo en los tiempos de conversión, los tiempos de carga y también los costos de actualización por fotograma en tiempo de ejecución.

* Si la aplicación no usa [planos de corte](../../overview/features/cut-planes.md), la marca `opaqueMaterialDefaultSidedness` debe desactivarse. La ganancia de rendimiento suele situarse entre un 20 y un 30 %. Se pueden seguir usando los planos de corte, pero no habrá caras posteriores al fijarse en las partes internas de los objetos, lo que parece contradictorio. Para más información, consulte [Representación de un solo lado](../../overview/features/single-sided-rendering.md).

### <a name="use-case-photogrammetry-models"></a>Caso de uso: Modelos de fotogrametría

Al representar los modelos de fotogrametría, no suele ser necesario un gráfico de escena, por lo que puede establecer el `sceneGraphMode` en `none`. Sin embargo, dado que esos modelos raramente contienen un gráfico de escena complejo para comenzar, el impacto de esta opción debería ser insignificante.

Dado que la iluminación ya recurre a las texturas, no se necesita iluminación dinámica. Por lo tanto:

* Establezca la marca `unlitMaterials` en `true` para convertir todos los materiales en [materiales de color](../../overview/features/color-materials.md) sin iluminación.
* Quite los datos innecesarios del formato de vértice. Consulte el [ejemplo](#example) anterior.

### <a name="use-case-visualization-of-compact-machines-etc"></a>Caso de uso: Visualización de máquinas compactas, etc.

En estos casos de uso, los modelos suelen ser muy detallados en un volumen pequeño. El representador está muy optimizado para controlar estos casos de manera adecuada. Sin embargo, la mayoría de las optimizaciones mencionadas en el caso de uso anterior no se aplican aquí:

* Las partes individuales deben poder seleccionarse y moverse, por lo que el `sceneGraphMode` debe dejarse en `dynamic`.
* Las conversiones de rayo suelen ser una parte integral de la aplicación, por lo que se deben generar mallas de colisión.
* Los planos de corte se ven mejor con la marca `opaqueMaterialDefaultSidedness` habilitada.

## <a name="next-steps"></a>Pasos siguientes

* [Conversión de modelos](model-conversion.md)
* [Materiales de color](../../overview/features/color-materials.md)
* [Materiales de PBR](../../overview/features/pbr-materials.md)
* [Reemplazo de materiales durante la conversión de modelos](override-materials.md)

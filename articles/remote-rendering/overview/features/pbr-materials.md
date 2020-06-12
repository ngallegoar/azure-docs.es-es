---
title: Materiales de PBR
description: Describe el tipo de material de PBR.
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: e4ee6abe7481fef4d56c980da80e319624975384
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84021320"
---
# <a name="pbr-materials"></a>Materiales de PBR

Los *materiales de PBR* son uno de los [tipos de material](../../concepts/materials.md) admitidos en Azure Remote Rendering. Se usan para [mallas](../../concepts/meshes.md) que deben recibir una iluminación realista.

PBR son las siglas de **P**hysically **B**ased **R**endering y significan que el material describe las propiedades visuales de una superficie de forma física plausible, de modo que se puedan obtener resultados realistas en todas las condiciones de iluminación. La mayoría de los motores de juegos y las herramientas de creación de contenido actuales admiten materiales de PBR porque se consideran la mejor aproximación de escenarios del mundo real para la representación en tiempo real.

![Modelo de ejemplo glTF de casco representado por ARR](media/helmet.png)

Sin embargo, los materiales de PBR no son una solución universal. Hay materiales que reflejan el color de manera diferente en función del ángulo de visualización. Por ejemplo, algunos tejidos o pinturas de automóvil. Estos tipos de materiales no se controlan mediante el modelo PBR estándar y actualmente no se admiten en Azure Remote Rendering. Se incluyen las extensiones de PBR, como *Thin-Film* (superficies de varias capas) y *Clear-Coat* (para pinturas de automóvil).

## <a name="common-material-properties"></a>Propiedades de materiales comunes

Estas propiedades son comunes para todos los materiales:

* **albedoColor:** este color se multiplica con otros colores, como *albedoMap* o los *colores del :::no-loc text="vertex ":::* . Si el elemento *transparency* está habilitado en un material, el canal alfa se usa para ajustar la opacidad. En este contexto, `1` significa totalmente opaco y `0`, completamente transparente. El color predeterminado es el blanco.

  > [!NOTE]
  > Cuando un material de PBR es totalmente transparente, como un fragmento de vidrio limpio, sigue reflejando el entorno. Los puntos brillantes como el sol siguen siendo visibles en la reflexión. Esto es diferente para los [materiales de color](color-materials.md).

* **albedoMap:** una [textura en 2D](../../concepts/textures.md) para valores albedo por píxel.

* **alphaClipEnabled** y **alphaClipThreshold:** si *alphaClipEnabled* es true, no se dibujarán todos los píxeles en que el valor alfa de albedo sea inferior a *alphaClipThreshold*. El recorte alfa se puede usar incluso sin habilitar la transparencia y se representa mucho más rápidamente. Sin embargo, los materiales recortados alfa se siguen representando más lentamente que los materiales completamente opacos. De forma predeterminada, el recorte alfa está deshabilitado.

* **textureCoordinateScale** y **textureCoordinateOffset:** la escala se multiplica en las coordenadas de textura UV y se le agrega el desplazamiento. Se puede usar para ajustar y desplazar las texturas. La escala predeterminada es (1, 1) y el desplazamiento es (0, 0).

* **useVertexColor:** si la malla contiene colores de :::no-loc text="vertex"::: y esta opción está habilitada, el color de los :::no-loc text="vertex"::: de las mallas se multiplica en *albedoColor* y *albedoMap*. De forma predeterminada *useVertexColor* está deshabilitado.

* **isDoubleSided:** Si el valor de doble cara está establecido en true, los triángulos con este material se representan aunque la cámara apunte a sus caras posteriores. En el caso de la iluminación de materiales de PBR también se calcula correctamente para las caras posteriores. Esta opción está deshabilitada de manera predeterminada. Consulte también [Representación :::no-loc text="Single-sided":::](single-sided-rendering.md).

## <a name="pbr-material-properties"></a>Propiedades de los materiales de PBR

La idea principal de la representación basada en la física es usar las propiedades *BaseColor*, *Metalness*y *Roughness* para simular una amplia gama de materiales reales. Una descripción detallada de PBR queda fuera del ámbito de este artículo. Para obtener más información acerca de PBR, consulte [otras fuentes](http://www.pbr-book.org). Las propiedades siguientes son específicas de los materiales de PBR:

* **baseColor:** en los materiales de PBR, el *color de albedo* se conoce como *color base*. En Azure Remote Rendering, la propiedad de *color de albedo* ya está presente a través de las propiedades de materiales comunes, por lo que no hay ninguna propiedad de color base adicional.

* **roughness** y **roughnessMap:** roughness define el grado de rugosidad o suavidad de la superficie. Las superficies rugosas dispersan la luz en más direcciones que las superficies suaves, lo que hace que las reflexiones sean borrosas en lugar de nítidas. El intervalo de valores es de `0.0` a `1.0`. Si `roughness` es igual a `0.0`, los reflejos serán nítidos. Si `roughness` es igual a `0.5`, los reflejos serán borrosos.

  Si se proporcionan un valor de roughness y un mapa de roughness, el valor final será el producto de ambos.

* **metalness** y **metalnessMap:** En física, esta propiedad se corresponde con la naturaleza conductiva o dieléctrica de una superficie. Los materiales conductivos tienen propiedades reflectantes diferentes y tienden a ser reflectantes sin ningún color de albedo. En los materiales de PBR, esta propiedad afecta a la cantidad de superficie que refleja el entorno circundante. Los valores van de `0.0` a `1.0`. Si el valor de metalness es `0.0`, el color de albedo es totalmente visible y el material parece plástico o cerámica. Si el valor de metalness es `0.5`, parece metal pintado. Si el valor de metalness es `1.0`, la superficie pierde casi por completo el color de albedo y solo refleja el entorno. Por ejemplo, si `metalness` es `1.0` y `roughness` es `0.0`, una superficie parece el reflejo real.

  Si se proporcionan un valor de metalness y un mapa de metalness, el valor final será el producto de ambos.

  ![metalness y roughness](./media/metalness-roughness.png)

  En la imagen anterior, la esfera de la esquina inferior derecha es similar a un material de metal real y la parte inferior izquierda parece cerámica o plástico. El color de albedo también cambia según las propiedades físicas. Con una rugosidad mayor, el material pierde la nitidez de la reflexión.

* **normalMap:** para simular detalles específicos, se puede proporcionar un [mapa normal](https://en.wikipedia.org/wiki/Normal_mapping).

* **occlusionMap** y **aoScale:** la [oclusión ambiente](https://en.wikipedia.org/wiki/Ambient_occlusion) permite que los objetos con grietas parezcan más realistas al agregar sombras a las áreas ocluidas. Los valores de oclusión van de `0.0` a `1.0`, donde `0.0` significa oscuridad (oclusión) y `1.0` significa que no hay ninguna oclusión. Si se proporciona una textura 2D como un mapa de oclusión, el efecto se habilita y *aoScale* actúa como un multiplicador.

  ![Mapa de oclusión](./media/boom-box-ao2.gif)

* **transparent**: para los materiales de PBR, solo existe una opción de transparencia (habilitada o deshabilitada). La opacidad se define mediante el canal alfa del color de albedo. Cuando está habilitada, se invoca una canalización de representación más compleja para dibujar superficies semitransparentes. Azure Remote Rendering implementa la [transparencia independiente del orden](https://en.wikipedia.org/wiki/Order-independent_transparency) (OIT) real.

  La representación de geometría transparente es costosa. Si solo necesita orificios en una superficie, por ejemplo para las hojas de un árbol, es mejor usar el recorte alfa en su lugar.

  ![Transparencia](./media/transparency.png) Observe en la imagen anterior que la esfera más a la derecha es completamente transparente, pero la reflexión sigue siendo visible.

  > [!IMPORTANT]
  > Si se prevé cambiar un material de opaco a transparente en tiempo de ejecución, el representador debe utilizar el [modo de representación](../../concepts/rendering-modes.md) *TileBasedComposition*. Esta limitación no se aplica a los materiales que se convierten, como los materiales transparentes con los que se empieza.

## <a name="technical-details"></a>Detalles técnicos

Azure Remote Rendering usa el BRDF de microfaceta Cook-Torrance con GGX NDF, Schlick Fresnel y un término de visibilidad correlacionado de GGX Smith con un término difuso de Lambert. Este modelo es el estándar del sector de facto en este momento. Para obtener información más detallada, consulte este artículo: [(Representación basada en la física: Cook Torrance)](http://www.codinglabs.net/article_physically_based_rendering_cook_torrance.aspx)

 Una alternativa al modelo de PBR *Metalness-Roughness* que se usa en Azure Remote Rendering es el modelo de PBR *Specular-Glossiness*. Este modelo puede representar una gama más amplia de materiales. Sin embargo, es más caro y normalmente no funciona bien para los casos en tiempo real.
No siempre es posible realizar la conversión de *Specular-Glossiness* a *Metalness-Roughness*, ya que existen pares de valores *(Diffuse, Specular)* que no se pueden convertir a *(BaseColor, Metalness)* . La conversión en la otra dirección es más sencilla y más precisa, ya que todos los pares de *(BaseColor, Metalness)* se corresponden con los pares *(Diffuse, Specular)* bien definidos.

## <a name="next-steps"></a>Pasos siguientes

* [Materiales de color](color-materials.md)
* [Texturas](../../concepts/textures.md)
* [Mallas](../../concepts/meshes.md)

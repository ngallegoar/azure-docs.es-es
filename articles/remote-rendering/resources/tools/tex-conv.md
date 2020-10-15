---
title: 'TexConv: herramienta de conversión textura'
description: Manual del usuario para la herramienta de línea de comandos TexConv
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 1d9b2ca163b70435a6c0e245e66492e8e2866639
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80678842"
---
# <a name="texconv---texture-conversion-tool"></a>TexConv: herramienta de conversión textura

TexConv es una herramienta de línea de comandos para procesar texturas de formatos de entrada típicos, tales como PNG, TGA, JPEG y DDS, en formatos optimizados para el consumo en tiempo de ejecución.
Aunque el escenario más común consiste en convertir un solo archivo de entrada `A.xxx` a un formato optimizado `B.yyy`, la herramienta tiene muchas opciones adicionales para los usos avanzados.

## <a name="command-line-help"></a>Ayuda de la línea de comandos

Al ejecutar TexConv.exe con el parámetro `--help`, se enumerarán todas las opciones disponibles. Además, cuando la herramienta TexConv se ejecuta, imprime las opciones usadas para ayudar a comprender lo que está haciendo. Consulte esta salida para obtener más información.

## <a name="general-usage"></a>Uso general

TexConv siempre produce **un archivo de salida exactamente**. Puede usar **varios archivos de entrada** desde los que ensamblar la salida. Para el ensamblado, también necesita una **asignación de canales**, que le indica de qué canal (*rojo, verde, azul* o *alfa*) debe tomar el archivo de entrada y a qué canal debe moverlo de la imagen de salida.

La línea de comandos más directa es la siguiente:

```cmd
TexConv.exe -out D:/result.dds -in0 D:/img.jpg -rgba in0
```

- `-out` especifica el archivo de salida y el formato.
- `-in0` especifica la primera imagen de entrada.
- `-rgba` indica que la imagen de salida debe utilizar los cuatro canales y que deben tomarse con una relación de 1:1 de la imagen de entrada.

## <a name="multiple-input-files"></a>Varios archivos de entrada

Para ensamblar la salida de varios archivos de entrada, especifique cada archivo de entrada mediante la opción `-in` con un número creciente:

```cmd
-in0 D:/img0.jpg -in1 D:/img1.jpg -in2 D:/img2.jpg ...
```

Al ensamblar un mapa de cubo a partir de texturas 2D, también puede usar `-right`, `-left`, `-top`, `-bottom`, `-front`, `-back` o `-px`, `-nx`, `-py`, `-ny`, `-pz`, `-nz`.

Para asignar estas entradas al archivo de salida, se necesita una asignación de canal correcta.

## <a name="channel-mappings"></a>Asignaciones de canales

Las opciones de asignación de canales especifican la entrada desde la que se rellenarán los canales de salida especificados. Puede especificar la entrada para cada canal individualmente de la siguiente manera:

```cmd
-r in0.b -g in0.g -b in0.r -a in1.r
```

Aquí, los canales RGB de la salida se rellenarán con la primera imagen de entrada, pero los colores rojo y el azul se intercambiarán. El canal alfa de la salida se rellenará con los valores del canal rojo de la segunda imagen de entrada.

La especificación de la asignación de cada canal por separado ofrece la máxima flexibilidad. Para mayor comodidad, se puede escribir lo mismo con los operadores de "referenciación":

```cmd
-rgb in0.bgr -a in1.r
```

### <a name="output-channels"></a>Canales de salida

Están disponibles las siguientes opciones de asignación de canales:

- `-r`, `-g`, `-b`, `-a`: especifican las asignaciones de un solo canal
- `-rg`: especifica las asignaciones de los canales rojo y verde.
- `-rgb`: especifica las asignaciones de los canales rojo, verde y azul.
- `-rgba`: especifica las asignaciones de los cuatro canales.

Al mencionar solo el canal R, RG o RGB, se indica a TexConv que cree un archivo de salida con solo 1, 2 o 3 canales, respectivamente.

### <a name="input-swizzling"></a>Referenciación de entrada

Al indicar qué textura de entrada debe llenar el canal de salida, se puede referenciar la entrada:

- `-rgba in0` equivale a `-rgba in0.rgba`.
- `-rgba in0.bgra` referenciará los canales de entrada.
- `-rgb in0.rrr` duplicará el canal rojo en todos los canales.

También puede rellenar los canales con negro o blanco:

- `-rgb in0 -a white` creará una textura de salida de 4 canales, pero establecerá el canal alfa en totalmente opaco.
- `-rg black -b white` creará una textura completamente azul.

## <a name="common-options"></a>Opciones comunes

Las opciones más interesantes se indican a continuación. En `TexConv --help` se indican más opciones.

### <a name="output-type"></a>Tipo de salida

- `-type 2D`: la salida será una imagen 2D normal.
- `-type Cubemap`: la salida será una imagen de mapa de cubo. Solo se admite para los archivos de salida DDS. Cuando se especifica esta opción, se puede ensamblar el mapa de cubo a partir de las seis imágenes de entrada 2D normales.

### <a name="image-compression"></a>Compresión de imagen

- `-compression none`: la imagen de salida se descomprimirá.
- `-compression medium`: si se admite, la imagen de salida usará la compresión sin sacrificar en exceso la calidad.
- `-compression high`: si se admite, la imagen de salida usará la compresión y sacrificará la calidad en favor de un archivo más pequeño.

### <a name="mipmaps"></a>Mapas MIP

De forma predeterminada, TexConv genera mapas MIP si el formato de salida lo admite.

- `-mipmaps none`: no se generarán mapas MIP.
- `-mipmaps Linear`: si se admite, se generarán mapas MIP mediante un filtro de cuadro.

### <a name="usage-srgb--gamma-correction"></a>Uso (corrección sRGB/gamma)

La opción `-usage` especifica el propósito de la salida y, por tanto, indica a TexConv si se debe aplicar la corrección gamma a los archivos de entrada y salida. El uso solo afecta a los canales RGB. Siempre se considera que el canal alfa contiene valores "lineales". Si no se especifica el uso, el modo "automático" intentará detectar el uso a partir del formato y el nombre de archivo de la primera imagen de entrada. Por ejemplo, los formatos de salida de canal simple y doble son siempre lineales. Compruebe la salida para ver qué decisión ha tomado TexConv.

- `-usage Linear`: la imagen de salida contiene valores que no representan colores. Este suele ser el caso de las texturas metálicas y rugosas, así como de todos los tipos de máscaras.

- `-usage Color`: la imagen de salida representa el color, como las asignaciones difusas o de albedo. La marca sRGB se establecerá en el encabezado DDS de salida.

- `-usage HDR`: el archivo de salida debe usar más de 8 bits por píxel para la codificación. Por consiguiente, todos los valores se almacenan en un espacio lineal. En el caso de las texturas HDR, no importa si los datos representan el color u otros datos.

- `-usage NormalMap`: la imagen de salida representa un mapa normal de espacio tangente. Los valores se normalizarán y los cálculos de mapas MIP se optimizarán ligeramente.

- `-usage NormalMap_Inverted`: la salida es un mapa normal de espacio tangente con Y apuntando en la dirección opuesta a la entrada.

### <a name="image-rescaling"></a>Cambio de la escala de imagen

- `-minRes 64`: especifica la resolución mínima de la salida. Si la imagen de entrada es menor, se aumentará su escala.
- `-maxRes 1024`: especifica la resolución máxima de la salida. Si la imagen de entrada es mayor, se reducirá su escala.
- `-downscale 1`: si el valor es mayor que 0, la solución de las imágenes de entrada se reducirá a la mitad N veces. Use esta opción para aplicar una reducción de calidad general.

## <a name="examples"></a>Ejemplos

### <a name="convert-a-color-texture"></a>Conversión de una textura de color

```cmd
TexConv.exe -out D:/diffuse.dds -in0 D:/diffuse.jpg -rgba in0 -usage color
```

### <a name="convert-a-normal-map"></a>Conversión de un mapa normal

```cmd
TexConv.exe -out D:/normalmap.dds -in0 D:/normalmap.png -rgb in0 -usage normalmap
```

### <a name="create-an-hdr-cubemap"></a>Creación de un mapa de cubo HDR

```cmd
TexConv.exe -out "D:/skybox.dds" -in0 "D:/skymap.hdr" -rgba in0 -type cubemap -usage hdr
```

Una excelente fuente de mapas de cubo HDR es [hdrihaven.com](https://hdrihaven.com/hdris/).

### <a name="bake-multiple-images-into-one"></a>Integración de varias imágenes en una

```cmd
TexConv.exe -out "D:/Baked.dds" -in0 "D:/metal.tga" -in1 "D:/roughness.png" -in2 "D:/DiffuseAlpha.dds" -r in1.r -g in0.r -b black -a in2.a -usage linear
```

### <a name="extract-a-single-channel"></a>Extracción de un canal único

```cmd
TexConv.exe -out D:/alpha-mask-only.dds -in0 D:/DiffuseAlpha.dds -r in0.a
```

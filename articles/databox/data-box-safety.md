---
title: Seguridad de Azure Data Box | Microsoft Docs
description: Describe las consideraciones, directrices y convenciones de seguridad, y explica cómo instalar y operar con Azure Data Box de forma segura.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 08/13/2019
ms.author: alkohli
ms.openlocfilehash: db6e2eadbf19d78a203a4d4ba6111ad88430b192
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68985947"
---
# <a name="safely-install-and-operate-your-azure-data-box"></a>Instalación y operación con Azure Data Box de forma segura
![Icono Advertencia](./media/data-box-safety/warning_icon.png)
![Icono Leer el aviso de seguridad](./media/data-box-safety/read_safety_and_health_information_icon.png) **LEER LA INFORMACIÓN DE SEGURIDAD Y SALUD**

Lea toda la información de seguridad de este artículo antes de usar Azure Data Box. Si no sigue las instrucciones, podría provocar un incendio, descargas eléctricas u otros perjuicios o daños en las propiedades.

## <a name="safety-icon-conventions"></a>Convenciones de iconos de seguridad
Estos son los iconos que encontrará cuando revise las precauciones de seguridad que se deben observar al configurar y ejecutar Azure Data Box.

| Icono | Descripción |
|:--- |:--- |
| ![Icono Peligro](./media/data-box-safety/warning_icon.png) **PELIGRO** |Indica una situación peligrosa que, si no se evita, causará la muerte o lesiones graves. Esta palabra de aviso se debe limitar a las situaciones más extremas. |
| ![Icono Advertencia](./media/data-box-safety/warning_icon.png) **ADVERTENCIA** |Indica una situación peligrosa que, si no se evita, podría causar la muerte o lesiones graves. |
| ![Icono Advertencia](./media/data-box-safety/warning_icon.png) **PRECAUCIÓN** |Indica una situación peligrosa que, si no se evita, podría causar lesiones leves o graves. |
| ![Icono Aviso](./media/data-box-safety/notice_icon.png) **AVISO:** |Indica información considerada importante, pero no relacionada con la peligrosidad. |
| ![Icono Descarga eléctrica](./media/data-box-safety/electrical_shock_hazard_icon.png) **Peligro de descarga eléctrica** |Alta tensión. |
| ![Icono Peso pesado](./media/data-box-safety/heavy_weight_hazard_icon.png) **Peso pesado** | |
| ![Icono No contiene piezas reparables por el usuario](./media/data-box-safety/no_user_serviceable_parts_icon.png) **No contiene piezas reparables por el usuario** |No acceder sin la formación adecuada. |
| ![Icono Leer el aviso de seguridad](./media/data-box-safety/read_safety_and_health_information_icon.png) **Lea todas las instrucciones primero** | |
| ![Icono Sugerencia peligrosa](./media/data-box-safety/tip_hazard_icon.png) **Sugerencia peligrosa** | |

## <a name="handling-precautions"></a>Precauciones en la manipulación

![Icono Advertencia](./media/data-box-safety/warning_icon.png) ![Icono Descarga eléctrica](./media/data-box-safety/electrical_shock_hazard_icon.png)![Icono No contiene piezas reparables por el usuario](./media/data-box-safety/no_user_serviceable_parts_icon.png) **PRECAUCIÓN** 

* Inspeccione el dispositivo *recibido* en busca de daños. Si el contenedor del dispositivo está dañado, [póngase en contacto con el Soporte técnico de Microsoft](data-box-disk-contact-microsoft-support.md) para obtener un reemplazo. No intente hacer funcionar el dispositivo. 
* El dispositivo está equipado con tornillos a prueba de manipulaciones. Si sospecha que el dispositivo funciona mal, [póngase en contacto con el Soporte técnico de Microsoft](data-box-disk-contact-microsoft-support.md) para obtener un reemplazo. No intente reparar el dispositivo. 
* El dispositivo contiene piezas que el usuario no puede reparar. Dentro, hay elementos con niveles de energía, corriente y voltaje peligrosos. No lo abra. Devuelva el dispositivo a Microsoft para que lo repare.

![Icono Advertencia](./media/data-box-safety/warning_icon.png) ![Icono Peso pesado](./media/data-box-safety/heavy_weight_hazard_icon.png) **ADVERTENCIA** 

* Un contenedor configurado completamente puede pesar hasta 22,7 kg; no intente levantarlo sin ayuda.
* Antes de mover el alojamiento, asegúrese siempre de que sean dos personas las que levanten su peso. Tenga en cuenta que si una persona sola intenta levantar su peso, podría lesionarse.


![Icono Advertencia](./media/data-box-safety/warning_icon.png) ![Icono Sugerencia peligrosa](./media/data-box-safety/tip_hazard_icon.png) **ADVERTENCIA**
* Coloque el dispositivo en una superficie plana, dura y estable para evitar un posible riesgo de vuelco.
* El equipo montado en bastidor no está destinado a usarse como estantería ni como espacio de trabajo. No coloque el dispositivo Data Box sobre el equipo montado en bastidor. Agregar cualquier tipo de carga a una unidad montada en bastidor extendida puede crear un riesgo potencial de vuelco que podría provocar daños en el producto, lesiones o la muerte.

![Icono Advertencia](./media/data-box-safety/warning_icon.png) **ADVERTENCIA**

* Configure el dispositivo en un área de trabajo dejando un espacio adecuado para la circulación de aire alrededor del dispositivo.
* Instale el dispositivo en un área interior con control de la temperatura y libre de contaminantes conductores, y dejando un espacio adecuado para la circulación de aire alrededor del dispositivo.
* Mantenga el dispositivo fuera de fuentes de líquidos y de entornos excesivamente húmedos.


## <a name="electrical-precautions"></a>Precauciones eléctricas

![Icono Advertencia](./media/data-box-safety/warning_icon.png) ![icono Descarga eléctrica](./media/data-box-safety/electrical_shock_hazard_icon.png) **ADVERTENCIA**

* Proporcione una conexión eléctrica a tierra segura para el cable de la fuente de alimentación. El cable de CA tiene una clavija de tres hilos (un enchufe con un pin de toma de tierra). Esta clavija solo encaja en un enchufe con toma de tierra. No anule el propósito de la clavija de toma de tierra.
* Dado que la clavija del cable de la fuente de alimentación es el dispositivo de desconexión principal, asegúrese de que los enchufes están ubicados cerca del dispositivo y que se pueda acceder a ellos con facilidad.
* Desconecte el cable de alimentación (tirando de la clavija, no del cable) y desconecte todos los cables, si se diera alguna de las condiciones siguientes:

    - El cable de alimentación o la clavija se deshilacha o se daña de alguna otra forma.
    - Derrama líquido en la carcasa del dispositivo.
    - El dispositivo se expone a la lluvia o a exceso de humedad.
    - El dispositivo se cayó y su carcasa está dañada.
    - Sospecha que el dispositivo necesita ser reparado.
* Desenchufe de forma definitiva la unidad antes de moverla o si piensa que se ha dañado de algún modo.
* Proporcione una fuente de alimentación adecuada con protección frente a sobrecargas eléctricas para cumplir con las especificaciones de energía siguientes:

    - Voltaje: de 100 V CA a 240 V CA
    - Corriente: 6 A como máximo
    - Frecuencia: de 50 Hz a 60 Hz

![Icono Advertencia](./media/data-box-safety/warning_icon.png) **PRECAUCIÓN:**

* Este dispositivo contiene baterías de celda de botón. No intente reparar el dispositivo. El usuario no puede reparar las baterías de este dispositivo. 
* **Solo para personal de mantenimiento**: Hay riesgo de explosión si la batería se sustituye por una de un tipo incorrecto. Deseche las baterías usadas según las instrucciones.

![Icono Aviso](./media/data-box-safety/notice_icon.png) **AVISO:**

Para una operación apropiada del dispositivo y para evitar daños en el producto:

* Asegúrese de que la parte frontal y las puertas traseras están totalmente abiertas mientras el dispositivo está en funcionamiento.

## <a name="regulatory-information"></a>Información de disposiciones legales

Esta sección contiene información de disposiciones legales para Azure Data Box, con el número de modelo de disposiciones legales DB010.

Este dispositivo es:

- Evaluado como Equipo de tecnologías de la información (ITE) diseñado para funcionar en un entorno de espacio de datos típico. La idoneidad de este producto para otros entornos puede requerir una evaluación adicional.
- Diseñado para usarse con equipos de tecnologías de información conforme con IEC/EN 60950-1 o IEC/EN 62368 1 (marca CE), y NRTL enumerados (UL, CSA, ETL, etc.).
- Diseñado para funcionar en el siguiente entorno. 
    - Temperatura de funcionamiento: 10 °C a 35 °C
    - Temperatura de almacenamiento: -4 ° a 122 ° F (-20° a 50° C)
    - Humedad relativa: 15 % a 85 % (sin condensación) 
    - Altitud de funcionamiento: Probado desde 0 a 2000 metros

Para las clasificaciones de suministro eléctrico, consulte la etiqueta de clasificación de dispositivo proporcionada con la unidad. 

![Icono Aviso](./media/data-box-safety/notice_icon.png) **AVISO:** 

Las modificaciones o cambios realizados en el dispositivo que no hayan sido aprobados expresamente por Microsoft podrían anular la autorización del usuario para operar con el dispositivo.

**CANADÁ y EE. UU.:**

![Icono Aviso](./media/data-box-safety/notice_icon.png) **AVISO:** 

Este equipo ha sido probado y cumple con límites de un dispositivo digital de Clase A, en virtud de la sección 15 de las Normas de FCC. Estos límites están diseñados para proporcionar una protección razonable contra interferencias perjudiciales cuando el equipo se hace funcionar en un entorno comercial. Este equipo genera, utiliza y puede irradiar energía de radiofrecuencia y, si no se instala y se usa de acuerdo con el manual de instrucciones, puede causar interferencias perjudiciales para las comunicaciones de radio. El uso de este equipo en una zona residencial puede provocar interferencias perjudiciales, en cuyo caso, se requerirá que el usuario corrija la interferencia a su costa.

Este dispositivo cumple con la sección 15 de las Normas de FCC y los estándares RSS exentos de licencia de Industry Canada. El uso está sujeto a las dos condiciones siguientes: (1) este dispositivo no puede causar interferencias perjudiciales y (2) este dispositivo debe aceptar cualquier interferencia recibida, incluida una que pueda ocasionar el mal funcionamiento del dispositivo.

![Canadá](./media/data-box-safety/canada.png)

CAN ICES-3(A)/NMB-3(A)

Microsoft Corporation, One Microsoft Way, Redmond, WA 98052, EE.UU.

Estados Unidos: (800) 426-9400

Canadá: (800) 933-4750

**UNIÓN EUROPEA:**

[Solicite una copia de la declaración de conformidad de la UE](mailto:CSI_Compliance@microsoft.com).

![Icono Advertencia](./media/data-box-safety/warning_icon.png) **ADVERTENCIA:** 

Se trata de un producto de clase A. En un entorno doméstico, este producto puede causar interferencias de radio, en cuyo caso es posible que se pida al usuario que tome las medidas adecuadas.

**Eliminación de residuos de baterías y aparatos eléctricos y electrónicos:**

![Icono de eliminación de la batería](./media/data-box-safety/battery_disposal_icon.png)

Este símbolo en el producto o en su embalaje o baterías significa que este producto y las baterías que contiene no se deben eliminar con los residuos domésticos. En su lugar, es responsabilidad suya entregarlas a un punto de recolección aplicable para el reciclaje de baterías y aparatos eléctricos y electrónicos. Esta recolección y reciclaje independiente ayudará a conservar los recursos naturales y a evitar posibles consecuencias desfavorables para la salud y el entorno debido a la posible presencia de sustancias peligrosas en las baterías y en los equipos eléctricos y electrónicos, que podrían ser ocasionados por una eliminación inapropiada. Para más información acerca de dónde dejar las baterías y los residuos eléctricos y electrónicos, póngase en contacto con la oficina local de su ciudad o municipio, el servicio doméstico de eliminación de deshechos o la tienda donde compró este producto. Póngase en contacto con *erecycle\@microsoft.com* para obtener más información sobre la Directiva de Residuos de Aparatos Eléctricos y Electrónicos.

Este producto contiene baterías de celda de moneda.

Microsoft Irlanda Sandyford Ind Est Dublín D18 KX32 IRL

Número de teléfono: +353 1 295 3826

Número de faz: +353 1 706 4110 

**Japón**

![Japón](./media/data-box-safety/japan.png)

<!--**South Korea**

![South Korea](./media/data-box-safety/south-korea.png)

**China RoHS Hazardous Substances Table**

![China RoHS hazardous substances](./media/data-box-safety/south-korea.png)

**Taiwan**

![Taiwan](./media/data-box-safety/taiwan.png)-->

Después de revisar estos avisos de seguridad, pude configurar y cablear su dispositivo.

## <a name="next-steps"></a>Pasos siguientes

* [Cable y conexión de Data Box](data-box-deploy-set-up.md)



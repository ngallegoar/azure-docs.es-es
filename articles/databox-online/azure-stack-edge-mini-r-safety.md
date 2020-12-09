---
title: Guía de seguridad de Azure Stack Edge Mini R | Microsoft Docs
description: En este artículo se describen las consideraciones, directrices y convenciones de seguridad y se explica cómo instalar y usar el dispositivo de Azure Stack Edge Mini R de manera segura.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 10/13/2020
ms.author: alkohli
ms.openlocfilehash: 507ceef0f13951eafdcb02d586f35c1d61764c4e
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465837"
---
# <a name="azure-stack-edge-mini-r-safety-instructions"></a>Instrucciones de seguridad de Azure Stack Edge Mini R

![Icono de advertencia 1](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
![Icono Leer el aviso de seguridad](./media/azure-stack-edge-mini-r-safety/icon-safety-read-all-instructions.png)
 **LEA LA INFORMACIÓN DE SEGURIDAD Y SALUD**

Lea toda la información de seguridad de este artículo antes de usar su dispositivo de Azure Stack Edge Mini R, que consta de una batería, una fuente de alimentación con conexión de CA/CC, un adaptador de alimentación y un módulo de servidor. Si no sigue las instrucciones, podría provocar un incendio, descargas eléctricas, daños o perjuicios en las propiedades. Lea toda la información de seguridad a continuación antes de usar el Azure Stack Edge Mini R.

## <a name="safety-icon-conventions"></a>Convenciones de iconos de seguridad

Las siguientes palabras de advertencia para los símbolos de notificación de peligro son:

| Icono | Descripción |
|:--- |:--- |
| ![Símbolo de peligro](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)| **PELIGRO:** Indica una situación peligrosa que, si no se evita, causará la muerte o lesiones graves. <br> **ADVERTENCIA:** Indica una situación peligrosa que, si no se evita, podría causar la muerte o lesiones graves. <br> **PRECAUCIÓN:** Indica una situación peligrosa que, si no se evita, podría causar lesiones leves o graves.|
|

Los siguientes iconos de riesgo se deben tener en mente al configurar y poner en marcha el dispositivo de Azure Stack Edge Mini R:

| Icono | Descripción |
|:--- |:--- |
| ![Lea todas las instrucciones primero](./media/azure-stack-edge-mini-r-safety/icon-safety-read-all-instructions.png) | Lea todas las instrucciones primero |
| ![Símbolo de peligro](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) | Símbolo de peligro |
| ![Icono de descarga eléctrica](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) | Peligro de descarga eléctrica |
| ![Solo para uso en interiores](./media/azure-stack-edge-mini-r-safety/icon-safety-indoor-use-only.png) | Solo para uso en interiores |
| ![Icono de que no contiene piezas reparables por el usuario](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) | No contiene piezas reparables por el usuario. No acceder sin la formación adecuada. |
|

## <a name="handling-precautions-and-site-selection"></a>Precauciones de manejo y selección de ubicación

El dispositivo de Azure Stack Edge Mini R cuenta con las siguientes precauciones de manejo y criterios de selección de ubicación:

![Icono de advertencia 2](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
![Icono de descarga eléctrica](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png)
![Icono No contiene piezas reparables por el usuario](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) **PRECAUCIÓN:**

* Inspeccione el dispositivo *recibido* en busca de daños. Si el contenedor del dispositivo está dañado, [póngase en contacto con el Soporte técnico de Microsoft](azure-stack-edge-placeholder.md) para obtener un reemplazo. No intente hacer funcionar el dispositivo.
* Si sospecha que el dispositivo funciona mal, [póngase en contacto con el Soporte técnico de Microsoft](azure-stack-edge-placeholder.md) para obtener un reemplazo. No intente reparar el dispositivo.
* El dispositivo contiene piezas que el usuario no puede reparar. Dentro, hay elementos con niveles de energía, corriente y voltaje peligrosos. No lo abra. Devuelva el dispositivo a Microsoft para que lo repare.

![Icono de advertencia 3](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **PRECAUCIÓN:**

Se recomienda usar el sistema de la siguiente manera:

* Alejado de fuentes de calor, incluidos radiadores y luz solar directa.
* En lugares que no estén expuestos a humedad o lluvia.
* En un espacio que reduzca la vibración y los impactos físicos.  El sistema está diseñado para soportar impactos y vibraciones de conformidad con MIL-STD-810G.
* Aislado de campos electromagnéticos potentes generados por dispositivos eléctricos.
* No permita que ningún líquido u objeto extraño entre en el sistema. No coloque bebidas ni otros contenedores de líquidos sobre el sistema o cerca de él.

![Icono de advertencia 4](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
![Icono No contiene piezas reparables por el usuario](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) **PRECAUCIÓN:**

* Este equipo contiene una batería de litio. No intente dar mantenimiento a la batería. El usuario no puede reparar las baterías de este equipo. Hay riesgo de explosión si la batería se sustituye por una de un tipo incorrecto.

![Icono de advertencia 5](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **PRECAUCIÓN:**

Solo debe cargar la batería cuando esté conectada al dispositivo de Azure Stack Edge Mini R, no debe cargarla como un dispositivo independiente.

![Icono de advertencia 6](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **PRECAUCIÓN:**

* El interruptor de encendido y apagado de la batería solo es para descargar la batería en el módulo de servidor. Si el adaptador de alimentación está conectado a la batería, la alimentación se pasará al módulo del servidor incluso si el interruptor está en la posición de apagado.

![Icono de advertencia 7](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **PRECAUCIÓN:**

* No queme ni provoque cortocircuitos a la batería. Se debe reciclar o desechar correctamente.

![Icono de advertencia 8](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **PRECAUCIÓN:**

* En lugar de usar la fuente de alimentación de CA/CC proporcionada, este sistema también tiene la opción de usar una batería de tipo 2590 comprobada en el terreno. En este caso, el usuario final debe comprobar que cumple todas las normativas relativas a la seguridad, el transporte, el medio ambiente y cualquier otra norma nacional o local.
* Cuando use el sistema con una batería de tipo 2590, use dicha batería según las condiciones de uso especificadas por el fabricante de la misma.

![Icono de advertencia 9](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **PRECAUCIÓN:**

Este dispositivo tiene dos puertos SFP + que se pueden usar con transceptores ópticos. Para evitar radiación láser peligrosa, use solo los transceptores de clase 1.

## <a name="electrical-precautions"></a>Precauciones eléctricas

El dispositivo de Azure Stack Edge Mini R tiene las siguientes precauciones eléctricas:

![Icono de advertencia 10](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) ![Icono de descarga eléctrica](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) **ADVERTENCIA:**

Cuando se use con el adaptador de fuente de alimentación:

* Proporcione una conexión eléctrica a tierra segura para el cable de la fuente de alimentación. El cable de corriente alterna (CA) tiene una clavija de tres cables (un enchufe con un pin de toma de tierra). Esta clavija solo encaja en un enchufe con toma de tierra. No anule el propósito de la clavija de toma de tierra.
* Dado que la clavija del cable de la fuente de alimentación es el dispositivo de desconexión principal, asegúrese de que los enchufes están ubicados cerca del dispositivo y que se pueda acceder a ellos con facilidad.
* Desconecte los cables de alimentación (tirando de la clavija, no del cable) y desconecte todos los cables, si se diera alguna de las condiciones siguientes:

  * El cable de alimentación o la clavija se deshilacha o se daña de alguna otra forma.
  * El dispositivo está expuesto a la lluvia, a exceso de humedad o a otros líquidos.
  * El dispositivo se cayó y se dañó la carcasa del dispositivo.
  * Sospecha que el dispositivo necesita ser reparado.
* Desenchufe de forma definitiva la unidad antes de moverla o si piensa que se ha dañado de algún modo.

* Proporcione una fuente de alimentación adecuada con protección frente a sobrecargas eléctricas para cumplir con las especificaciones de energía siguientes:

* Voltaje: 100 a 240 voltios CA
* Corriente: 1,7 amperios
* Frecuencia: de 50 Hz a 60 Hz

![Icono de advertencia 11](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
![Icono de descarga eléctrica](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) **ADVERTENCIA:**

* No intente modificar ni usar cables de alimentación de CA distintos a los que se proporcionan con el equipamiento.

![Icono de advertencia 12](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
![Icono de descarga eléctrica](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png)
![Solo para uso en interiores](./media/azure-stack-edge-mini-r-safety/icon-safety-indoor-use-only.png) **ADVERTENCIA:**

* Las fuentes de alimentación etiquetadas con este símbolo solo están clasificadas para uso en interiores.

## <a name="regulatory-information"></a>Información de disposiciones legales

A continuación se incluye información reglamentaria del dispositivo de Azure Stack Edge Mini R con el número de modelo normativo: TMA01.

El dispositivo de Azure Stack Edge Mini R está diseñado para usarse con equipos de tecnologías de información conforme con IEC/EN 60950-1 o IEC/EN 62368 1 (marca CE), y parte de la lista de NRTL (UL, CSA, ETL, etc.).

En los países distintos de EE. UU. y Canadá, los cables de red (no incluidos con el equipo) no se deben instalar con este equipo si su longitud supera los tres metros.

El equipo está diseñado para funcionar en los siguientes entornos:

| Entorno | Especificaciones |
|:---  |:--- |
| Especificaciones de temperatura del sistema | <ul><li>Temperatura de almacenamiento: -20 &deg;C a 50 &deg;C (-4 &deg;F a 122 &deg;F)</li><li>Operación continua: 0 &deg;C a 40 &deg;C (32 &deg;F a 104 &deg;F)</li></ul> |
| Especificaciones de humedad relativa | <ul><li>Almacenamiento: del 5 a 95 % de humedad relativa</li><li>Operativo: del 10 a 90 % de humedad relativa</li></ul>|
| Especificaciones de altitud máxima | <ul><li>Operativo: 15 000 pies (4572 metros)</li><li>No operativo: 40 000 pies (12 192 metros)</li></ul>|

> ![Icono Aviso](./media/azure-stack-edge-mini-r-safety/icon-safety-notice.png) **AVISO:** &nbsp;Las modificaciones o cambios realizados en el equipo que no hayan sido aprobados expresamente por Microsoft podrían anular la autorización del usuario para operar con el equipo.

CANADÁ y EE. UU.:

AVISO: Este equipo ha sido probado y cumple con límites de un dispositivo digital de Clase A, en virtud de la sección 15 de las Normas de FCC. Estos límites están diseñados para proporcionar una protección razonable contra interferencias perjudiciales cuando el equipo se hace funcionar en un entorno comercial. Este equipo genera, utiliza y puede irradiar energía de radiofrecuencia y, si no se instala y se usa de acuerdo con el manual de instrucciones, puede causar interferencias perjudiciales para las comunicaciones de radio. El uso de este equipo en una zona residencial puede provocar interferencias perjudiciales, en cuyo caso, se requerirá que el usuario corrija la interferencia a su costa.

Este dispositivo cumple con la sección 15 de las Normas de FCC y los estándares RSS exentos de licencia de Industry Canada. El uso está sujeto a las dos condiciones siguientes: (1) este dispositivo no puede causar interferencias perjudiciales y (2) este dispositivo debe aceptar cualquier interferencia recibida, incluida una que pueda ocasionar el mal funcionamiento del dispositivo.

![Advertencia de información sobre normativa 1](./media/azure-stack-edge-mini-r-safety/regulatory-information-1.png)

CAN ICES-3(A)/NMB-3(A) Microsoft Corporation, One Microsoft Way, Redmond, WA 98052, EE. UU.
Estados Unidos: (800) 426-9400 Canadá: (800) 933-4750

UNIÓN EUROPEA: Solicite una copia de la declaración de conformidad de la UE.

> ![Icono de advertencia 13](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) Se trata de un producto de clase A. En un entorno doméstico, este producto puede causar interferencias de radio, en cuyo caso es posible que se pida al usuario que tome las medidas adecuadas.

Eliminación de residuos de baterías y aparatos eléctricos y electrónicos:

![Icono de advertencia 14](./media/azure-stack-edge-mini-r-safety/icon-ewaste-disposal.png)

Este símbolo en el producto o en su embalaje o baterías significa que este producto y las baterías que contiene no se deben eliminar con los residuos domésticos. En su lugar, es responsabilidad suya entregarlas a un punto de recolección aplicable para el reciclaje de baterías y aparatos eléctricos y electrónicos. Esta recolección y reciclaje independiente ayudará a conservar los recursos naturales y a evitar posibles consecuencias desfavorables para la salud y el entorno debido a la posible presencia de sustancias peligrosas en las baterías y en los equipos eléctricos y electrónicos, que podrían ser ocasionados por una eliminación inapropiada. Para más información acerca de dónde dejar las baterías y los residuos eléctricos y electrónicos, póngase en contacto con la oficina local de su ciudad o municipio, el servicio doméstico de eliminación de deshechos o la tienda donde compró este producto. Póngase en contacto con erecycle@microsoft.com para más información de WEEE.

Este producto contiene baterías de celda de moneda.
Microsoft Irlanda Sandyford Ind Est Dublín D18 KX32 IRL Número de teléfono: +353 1 295 3826 Número de fax: +353 1 706 4110

## <a name="next-steps"></a>Pasos siguientes

- [Preparación de la implementación de Azure Stack Edge Mini R](azure-stack-edge-mini-r-deploy-prep.md)

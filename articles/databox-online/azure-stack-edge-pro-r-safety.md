---
title: Guía de seguridad de Azure Stack Edge Pro R | Microsoft Docs
description: En este artículo se describen las consideraciones, directrices y convenciones de seguridad y se explica cómo instalar y usar el dispositivo de Azure Stack Edge Pro R de manera segura.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 10/26/2020
ms.author: alkohli
ms.openlocfilehash: ac3f2cc1c68ea552b2858d932217a28055fee0fd
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465854"
---
# <a name="azure-stack-edge-pro-r-safety-instructions"></a>Instrucciones de seguridad de Azure Stack Edge Pro R

![Icono Advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png)
![Icono Leer el aviso de seguridad](./media/azure-stack-edge-pro-r-safety/icon-safety-read-all-instructions.png)
**LEER LA INFORMACIÓN DE SEGURIDAD Y SALUD**

Lea toda la información de seguridad de este artículo antes de usar el dispositivo de Azure Stack Edge Pro R. Si no sigue las instrucciones, podría provocar un incendio, descargas eléctricas, daños o perjuicios en las propiedades. Lea toda la información de seguridad a continuación antes de usar el Azure Stack Edge Pro R.

## <a name="safety-icon-conventions"></a>Convenciones de iconos de seguridad

Las siguientes palabras de advertencia para los símbolos de notificación de peligro son:

| Icono | Descripción |
|:--- |:--- |
| ![Símbolo de peligro](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png)| **PELIGRO:** Indica una situación peligrosa que, si no se evita, causará la muerte o lesiones graves. <br> **ADVERTENCIA:** Indica una situación peligrosa que, si no se evita, podría causar la muerte o lesiones graves. <br> **PRECAUCIÓN:** Indica una situación peligrosa que, si no se evita, podría causar lesiones leves o graves.|
|

Los siguientes iconos de riesgo se deben tener en mente al configurar y poner en marcha el dispositivo de Azure Stack Edge Pro R:

| Icono | Descripción |
|:--- |:--- |
| ![Lea todas las instrucciones primero](./media/azure-stack-edge-pro-r-safety/icon-safety-read-all-instructions.png) | Lea todas las instrucciones primero |
| ![Símbolo de peligro](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png) | Símbolo de peligro |
| ![Icono de sugerencia peligrosa](./media/azure-stack-edge-pro-r-safety/icon-safety-tip-hazard.png)  | Sugerencia peligrosa|
| ![Icono de pesado](./media/azure-stack-edge-pro-r-safety/icon-safety-heavy-weight.png)  | Peligro de peso elevado|
| ![Icono de descarga eléctrica](./media/azure-stack-edge-pro-r-safety/icon-safety-electric-shock.png) | Peligro de descarga eléctrica |
| ![Icono de que no contiene piezas reparables por el usuario](./media/azure-stack-edge-pro-r-safety/icon-safety-do-not-access.png) | No contiene piezas reparables por el usuario. No acceder sin la formación adecuada. |
| ![Icono de varias fuentes de alimentación](./media/azure-stack-edge-pro-r-safety/icon-safety-disconnect-all-power.png)  | Varias fuentes de alimentación. Desconecte todos los cables de alimentación para desconectar toda la alimentación eléctrica del equipo. |
| ![Icono de puntos de enganche](./media/azure-stack-edge-pro-r-safety/icon-pinching-points.png)  | Hay puntos de enganche presentes. |
| ![Icono de componentes o superficies calientes](./media/azure-stack-edge-pro-r-safety/icon-hot-component-surface.png)  | Indica componentes o superficies calientes. |
|

## <a name="handling-precautions-and-site-selection"></a>Precauciones de manejo y selección de ubicación

![Icono Advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png) **ADVERTENCIA:**

* Se debe usar equipamiento adecuado (por ejemplo, elevador) y equipamiento de protección personal, como guantes, al mover y manipular el dispositivo como se envía.

![Icono de advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png)![Icono Peligro de vuelco](./media/azure-stack-edge-pro-r-safety/icon-safety-tip-hazard.png) **ADVERTENCIA:**

* Coloque el equipamiento en una superficie plana, dura y estable a fin de evitar un posible riesgo de vuelco o aplastamiento.
* No apile más de dos dispositivos uno sobre otro.
* Asegúrese de que el sistema está bien sujeto antes de apilarlo.
* No reubique ni mueva dispositivos apilados.
* No apile dispositivos que estén recibiendo energía de cables externos.
* Asegúrese de que los cables de alimentación no se aplasten ni se dañen durante las operaciones de apilamiento.
* Los dispositivos no se deben usar como mesas o áreas de trabajo.

![Icono de advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png)
![Icono de descarga eléctrica](./media/azure-stack-edge-pro-r-safety/icon-safety-electric-shock.png)
![Icono No contiene piezas reparables por el usuario](./media/azure-stack-edge-pro-r-safety/icon-safety-do-not-access.png) **PRECAUCIÓN:**

* Inspeccione el dispositivo *recibido* en busca de daños. Si el contenedor del dispositivo está dañado, [póngase en contacto con el Soporte técnico de Microsoft](azure-stack-edge-contact-microsoft-support.md) para obtener un reemplazo. No intente hacer funcionar el dispositivo.
* Si sospecha que el dispositivo funciona mal, [póngase en contacto con el Soporte técnico de Microsoft](azure-stack-edge-contact-microsoft-support.md) para obtener un reemplazo. No intente reparar el dispositivo.
* El dispositivo contiene piezas que el usuario no puede reparar. Dentro, hay elementos con niveles de energía, corriente y voltaje peligrosos. No lo abra. Devuelva el dispositivo a Microsoft para que lo repare.

![Icono de advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png) ![Icono de peso elevado](./media/azure-stack-edge-pro-r-safety/icon-safety-heavy-weight.png) **ADVERTENCIA:**

* Al quitar el módulo de la fuente de alimentación del sistema de alimentación ininterrumpida, los componentes electrificados quedan expuestos. No inserte ningún objeto externo en el compartimiento del módulo de la fuente de alimentación.
* No intente levantar el dispositivo de Azure Stack Edge Pro R solo. Una carcasa puede pesar entre 52 kg y 93 kg (115 libras y 205 libras); haga uso de asistencia mecánica o de otro tipo para mover y levantar el equipo. Cumpla los requisitos de salud y seguridad laboral locales al mover y levantar el equipo.
* No intente levantar el equipo sin la ayuda mecánica adecuada. Tenga en cuenta que tratar de levantar este peso puede causar lesiones graves.

![Icono de advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png) **ADVERTENCIA:**

* El sistema está diseñado para funcionar en un entorno controlado. Elija un sitio que esté:
  * Bien ventilado y alejado de fuentes de calor, incluidos radiadores y luz solar directa.
  * Protegido de la humedad o la lluvia.
  * En un espacio que reduzca la vibración y los impactos físicos.  El sistema está diseñado para soportar impactos y vibraciones de conformidad con MIL-STD-810G.
  * Aislado de campos electromagnéticos potentes generados por dispositivos eléctricos.
  * Equipado con enchufes con toma de tierra.
  * Dotado de espacio adecuado para acceder a los cables de la fuente de alimentación, ya que sirven como desconexión de energía principal del producto.
* Configure el equipo en un área de trabajo que permita una adecuada circulación de aire alrededor del equipo; asegúrese de que las tapas delantera y trasera se hayan retirado completamente mientras el dispositivo se está usando.
* Instale el equipo en un área con control de la temperatura y libre de contaminantes conductores, y deje un espacio adecuado para la circulación de aire alrededor del equipo.
* Mantenga el equipo alejado de fuentes de líquidos y de entornos excesivamente húmedos.
* No permita que ningún líquido u objeto extraño entre en el sistema. No coloque bebidas ni otros contenedores de líquidos sobre el sistema o cerca de él.

## <a name="heater-precautions"></a>Precauciones de calefacción

![Icono de advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png) ![Icono de componentes o superficies calientes 1](./media/azure-stack-edge-pro-r-safety/icon-safety-electric-shock.png) **ADVERTENCIA:** 

* El funcionamiento automático del calentador mientras el sistema está encendido puede crear riesgos al tocarlo debido a las temperaturas elevadas de la superficie de la cubierta del conjunto de calentador. No toque esta superficie mientras el sistema está encendido. Deje que pase un período de enfriamiento de 10 minutos después de apagar el sistema.

![Icono de advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png) ![Icono de puntos de enganche 1](./media/azure-stack-edge-pro-r-safety/icon-safety-electric-shock.png) **ADVERTENCIA:** 

* Cuando el sistema está encendido, el accionamiento automático de la puerta de la cámara trasera puede crear un riesgo de punto de enganche. Aleje sus manos de esta área cuando el sistema está encendido.

## <a name="electrical-precautions"></a>Precauciones eléctricas

![Icono de advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png) ![Icono de descarga eléctrica](./media/azure-stack-edge-pro-r-safety/icon-safety-electric-shock.png) **ADVERTENCIA:**

* Proporcione una conexión eléctrica a tierra segura para el cable de la fuente de alimentación. El cable de corriente alterna (CA) tiene una clavija de tres cables (un enchufe con un pin de toma de tierra). Esta clavija solo encaja en un enchufe con toma de tierra. No anule el propósito de la clavija de toma de tierra.
* Dado que la clavija del cable de la fuente de alimentación es el dispositivo de desconexión principal, asegúrese de que los enchufes están ubicados cerca del dispositivo y que se pueda acceder a ellos con facilidad.
* Desconecte los cables de alimentación (tirando de la clavija, no del cable) y desconecte todos los cables, si se diera alguna de las condiciones siguientes:

  * El cable de alimentación o la clavija se deshilacha o se daña de alguna otra forma.
  * Derrama líquido en la carcasa del dispositivo.
  * El dispositivo se expone a la lluvia o a exceso de humedad.
  * El dispositivo se cayó y su carcasa está dañada.
  * Sospecha que el dispositivo necesita ser reparado.
* Desenchufe de forma definitiva la unidad antes de moverla o si piensa que se ha dañado de algún modo.
* Proporcione una fuente de alimentación adecuada con protección frente a sobrecargas eléctricas para cumplir con las especificaciones de energía siguientes:

  * Voltaje: 100 a 240 voltios CA
  * Corriente: 20 A máximo por cable de alimentación. Se incluyen los cables de alimentación.
  * Frecuencia: de 50 Hz a 60 Hz

![Icono de advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png)
![Icono de descarga eléctrica](./media/azure-stack-edge-pro-r-safety/icon-safety-electric-shock.png)
![Varias fuentes de energía](./media/azure-stack-edge-pro-r-safety/icon-safety-disconnect-all-power.png) **ADVERTENCIA:**

* En el caso de los sistemas sin un sistema de alimentación ininterrumpida (SAI), desconecte todos los cables de alimentación de CA para desconectar por completo la alimentación de CA del equipo.
* En el caso de los sistemas con un sistema de alimentación ininterrumpida, desconecte todos los cables de alimentación de CA y use el interruptor de alimentación de SAI para desactivar el sistema. El sistema de alimentación ininterrumpida contiene voltajes peligrosos de CA y CC.

![Icono de advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png)
![Icono de descarga eléctrica](./media/azure-stack-edge-pro-r-safety/icon-safety-electric-shock.png) **ADVERTENCIA:**

* En el caso de los sistemas equipados con un sistema de alimentación ininterrumpida, el voltaje de CA o CC siempre implica un riesgo potencial de voltaje de CA en la salida de SAI generada por las baterías o la utilidad. Para evitar daños al equipo o al personal, siempre debe suponer que puede haber voltaje en la salida de SAI, incluso cuando está desconectada de la fuente de alimentación principal.
* En el caso de los sistemas equipados con un sistema de alimentación ininterrumpida, todas las conexiones externas de SAI son hembras. No quite la carcasa ni inserte ningún objeto en estos conectores del SAI, ni en ningún otro.

![Icono de advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png)
![Icono de descarga eléctrica](./media/azure-stack-edge-pro-r-safety/icon-safety-electric-shock.png) **ADVERTENCIA:**

* No intente modificar ni usar cables de alimentación de CA distintos a los que se proporcionan con el equipamiento.

![Icono de advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png)
![Icono No contiene piezas reparables por el usuario](./media/azure-stack-edge-pro-r-safety/icon-safety-do-not-access.png) **PRECAUCIÓN:**

* Este equipo contiene baterías de célula de botón de litio o de fosfato de hierro de litio. No intente dar mantenimiento al equipo. El usuario no puede reparar las baterías de este equipo. Hay riesgo de explosión si la batería se sustituye por una de un tipo incorrecto.
* Al quitar el módulo de la batería del sistema de alimentación ininterrumpida, los componentes electrificados quedan expuestos. No inserte ningún objeto externo en el compartimiento del módulo de la batería.

## <a name="regulatory-information"></a>Información de disposiciones legales

Esta sección incluye información reglamentaria del dispositivo de Azure Stack Edge Pro R con el número de modelo normativo: Azure Stack Edge Pro R.

El dispositivo de Azure Stack Edge Pro R está diseñado para usarse con equipos de tecnologías de información conforme con IEC/EN 60950-1 o IEC/EN 62368 1 (marca CE), y parte de la lista de NRTL (UL, CSA, ETL, etc.).

El dispositivo está diseñado para funcionar en los siguientes entornos:

| Entorno | Especificaciones |
|:--- |:--- |
|Especificaciones de temperatura | <ul><li>Temperatura de almacenamiento: -33 &deg;C a 63 &deg;C (-28 &deg;F a 145 &deg;F) </li><li>Operación continua: 5 &deg;C a 43 &deg;C (41 &deg;F a 110 &deg;F)</li><li>Gradiente máximo de temperatura (funcionamiento y almacenamiento): 20 &deg;C/h (68 &deg;F/h)</li></ul> |
|Especificaciones de humedad relativa | <ul><li>Almacenamiento: del 5 al 95% de humedad relativa con 33 &deg;C (91 &deg;F) como punto de rocío máximo. La atmósfera debe encontrarse sin condensación en todo momento.</li><li>Operativo: del 5 al 85 % de humedad relativa con 29 &deg;C (84,2 &deg;F) como punto de rocío máximo</li></ul> |
| Especificaciones de altitud máxima | <ul><li>Operativo (sin fuente de alimentación): 15 000 pies (4572 metros)</li><li>Operativo (con fuente de alimentación): 6561 pies (2000 metros)</li><li>Almacenamiento: 40 000 pies (12 192 metros)</li></ul> |

<!--|Standard operating temperature specifications | <ul>Continuous operation (for altitude less than 950 m or 3117 ft): +5&deg;C–45&deg;C (41&deg;F–113&deg;F) with no direct sunlight on the equipment</ui>
|Expanded operating temperature specifications | <ul><li>Continuous operation: 5&deg;C to 45&deg;C at 5% to 85% RH with 29&deg;C dew point.</li><li></= 1% of annual operating hours: –5&deg;C to 55&deg;C at 5% to 90% RH with 29&deg;C dew point. |
| Expanded operating temperature restrictions | <ul><li>Do not perform cold start below -15&deg;C according to IEC 60945.</li><li>The operating temperature specified is for a maximum altitude of 950 meters.</li></ul> |
| Gaseous contamination specifications | <ul><li>Copper coupon corrosion rate: < 300 &Aring;/month per Class G1 as defined by ANSI/ISA71.04-1985.</li><li>Silver coupon corrosion rate: < 200 &Aring;/month as defined by AHSRAE TC9.9.</li></ul>|

> [!NOTE]
> Outside the standard operating temperature (10&deg;C to 35&deg;C):
> - The system can operate continuously in temperatures as low as 5&deg;C and as high as 45&deg;C. For temperatures between 35&deg;C and 45&deg;C, de-rate maximum allowable temperature by 1&deg;C per 175 m above 950 m (1&deg;F per 319 ft).
> - The system can operate down to –5&deg;C or up to 55&deg;C for a maximum of 1% of its annual operating hours. For temperatures between 45&deg;C and 55&deg;C, de-rate maximum allowable temperature by 1&deg;C per 125 m above 950 m (1&deg;F per 228 ft).
>
> When operating in the expanded temperature range:
> - System performance may be impacted.
> - Ambient temperature warnings may be reported in the System Event Log.     
>
> Maximum corrosive contaminant levels measured at &lt;/= 50% relative humidity. 


A device that has a UPS installed is designed to operate in the following environments:

| Environment | Specifications |
|:---  |:--- |
|Temperature specifications | <ul><li>Storage temperature: –40&deg;C–70&deg;C (–40&deg;F-158&deg;F) per Mil-Std 810G Method 501.5, Proc 1</li><li>Continuous operation (for altitude less than 950 m or 3117 ft): 5&deg;C–45&deg;C (41&deg;F–113&deg;F), with no direct sunlight on the equipment</li><li>Excursion temperature: 50&deg;C per Mil-Std 810G</li><li>Maximum temperature gradient (operating and storage): 20&deg;C/h (68&deg;F/h)</li></ul> |
|Relative humidity specifications | <ul><li>Storage: 5% to 95% RH with 33&deg;C (91&deg;F) maximum dew point. Atmosphere must be non-condensing at all times.</li><li>Operating: 5% to 85% relative humidity with 29&deg;C (84.2&deg;F) maximum dew point</li></ul>|
|Maximum altitude specifications | <ul><li>Storage: Mil-Std 810G method 500.5, Proc. I, 40,000 ft for 1 hour after stabilization </li><li>Operating: Mil-Std 810G method 500.5, Proc. II, air carriage, 15,000 ft for 1 hour after stabilization</li></ul>|
|Standard operating temperature specifications |<ul><li>Continuous operation (for altitude less than 950 m or 3117 ft)</li><li>+5&deg;C–45&deg;C (41&deg;F–113&deg;F) with no direct sunlight on the equipment</li></ul>|
|Expanded operating temperature specifications | <ul><li>Continuous operation: 5&deg;C to 45&deg;C at 5% to 85% RH with 29&deg;C dew point.</li><li>&lt;/= 1% of annual operating hours: 0&deg;C to 50&deg;C at 5% to 90% RH with 29&deg;C dew point.</li></ul>|
|Expanded operating temperature restrictions | <ul><li>Do not perform cold start below -15C Per IEC 60945</li><li>The operating temperature specified is for a maximum altitude of 950 meters</li></ul> |
|Gaseous contamination specifications | <ul><li>Copper coupon corrosion rate: < 300 &Aring;/month per Class G1 as defined by ANSI/ISA71.04-1985.</li><li>Silver coupon corrosion rate: < 200 &Aring;/month as defined by AHSRAE TC9.9.|-->

<!--
> [!NOTE]
> Outside the standard operating temperature (10&deg;C to 35&deg;C):
> - The system can operate continuously in temperatures as low as 5&deg;C and as high as 45&deg;C. For temperatures between 35&deg;C and 45&deg;C, de-rate maximum allowable temperature by 1&deg;C per 175 m above 950 m (1&deg;F per 319 ft).
> - The system can operate down to 0&deg;C or up to 50&deg;C for a maximum of 1% of its annual operating hours. For temperatures between 45&deg;C and 55&deg;C, de-rate maximum allowable temperature by 1&deg;C per 125 m above 950 m (1&deg;F per 228 ft).
> 
> When operating in the expanded temperature range:
> - System performance may be impacted.
> - Ambient temperature warnings may be reported in the System Event Log.
>
> Maximum corrosive contaminant levels measured at &lt;/= 50% relative humidity. --> 


> ![Icono Aviso](./media/azure-stack-edge-pro-r-safety/icon-safety-notice.png) **AVISO:** &nbsp;Las modificaciones o cambios realizados en el equipo que no hayan sido aprobados expresamente por Microsoft podrían anular la autorización del usuario para operar con el equipo.

CANADÁ y EE. UU.:

AVISO: Este equipo ha sido probado y cumple con los límites de un dispositivo digital de clase A, en virtud de la sección 15 de las Normas de FCC. Estos límites están diseñados para proporcionar una protección razonable contra interferencias perjudiciales cuando el equipo se hace funcionar en un entorno comercial. Este equipo genera, utiliza y puede irradiar energía de radiofrecuencia y, si no se instala y se usa de acuerdo con el manual de instrucciones, puede causar interferencias perjudiciales para las comunicaciones de radio. El uso de este equipo en una zona residencial puede provocar interferencias perjudiciales, en cuyo caso, se requerirá que el usuario corrija la interferencia a su costa.

Este dispositivo cumple con la sección 15 de las Normas de FCC y los estándares RSS exentos de licencia de Industry Canada. El uso está sujeto a las dos condiciones siguientes: (1) este dispositivo no puede causar interferencias perjudiciales y (2) este dispositivo debe aceptar cualquier interferencia recibida, incluida una que pueda ocasionar el mal funcionamiento del dispositivo.

![Advertencia de información sobre normativa 1](./media/azure-stack-edge-mini-r-safety/regulatory-information-1.png)


CAN ICES-3(A)/NMB-3(A) Microsoft Corporation, One Microsoft Way, Redmond, WA 98052, EE. UU.
Estados Unidos: (800) 426-9400 Canadá: (800) 933-4750

UNIÓN EUROPEA: Solicite una copia de la declaración de conformidad de la UE. 

![Icono de advertencia](./media/azure-stack-edge-pro-r-safety/icon-safety-warning.png)

Se trata de un producto de clase A. En un entorno doméstico, este producto puede causar interferencias de radio, en cuyo caso es posible que se pida al usuario que tome las medidas adecuadas.

Eliminación de residuos de baterías y aparatos eléctricos y electrónicos:

![Icono de advertencia 14](./media/azure-stack-edge-mini-r-safety/icon-ewaste-disposal.png)

Este símbolo en el producto o en su embalaje o baterías significa que este producto y las baterías que contiene no se deben eliminar con los residuos domésticos. En su lugar, es responsabilidad suya entregarlas a un punto de recolección aplicable para el reciclaje de baterías y aparatos eléctricos y electrónicos. Esta recolección y reciclaje independiente ayudará a conservar los recursos naturales y a evitar posibles consecuencias desfavorables para la salud y el entorno debido a la posible presencia de sustancias peligrosas en las baterías y en los equipos eléctricos y electrónicos, que podrían ser ocasionados por una eliminación inapropiada. Para más información acerca de dónde dejar las baterías y los residuos eléctricos y electrónicos, póngase en contacto con la oficina local de su ciudad o municipio, el servicio doméstico de eliminación de deshechos o la tienda donde compró este producto. Póngase en contacto con erecycle@microsoft.com para más información de WEEE.

Este producto contiene baterías de celda de moneda.

Microsoft Irlanda Sandyford Ind Est Dublín D18 KX32 IRL Número de teléfono: +353 1 295 3826 Número de fax: +353 1 706 4110



## <a name="next-steps"></a>Pasos siguientes

- [Preparación de la implementación de Azure Stack Edge Pro R](azure-stack-edge-pro-r-deploy-prep.md)

---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: include
ms.date: 09/18/2019
ms.author: azcspmt;ayshak;cynthn
ms.custom: include file
ms.openlocfilehash: 033ec0f4011cae657fc464849aac5c5d9a850907
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83343327"
---
Azure Compute ofrece tamaños de máquinas virtuales que están aislados para un tipo concreto de hardware y dedicados a un solo cliente.  Estos tamaños de máquina virtual son más adecuados para cargas de trabajo que requieren un alto grado de aislamiento de otros clientes como, por ejemplo, las cargas de trabajo que incluyen elementos como el cumplimiento normativo y los requisitos legales.  Los clientes también puede elegir subdividir aún más los recursos de estas máquinas virtuales aisladas mediante la [compatibilidad de Azure para máquinas virtuales anidadas](https://azure.microsoft.com/blog/nested-virtualization-in-azure/).

Usar un tamaño aislado garantiza que la máquina virtual será la única que se ejecute en esa instancia de servidor específica.  Las ofertas de máquinas virtuales aisladas actuales incluyen:
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms
* Standard_GS5
* Standard_G5
* Standard_DS15_v2 <sup>*</sup>
* Standard_D15_v2 <sup>*</sup>
* Standard_F72s_v2

<sup>*</sup>La garantía de aislamiento se retirará el 15 de mayo de 2020

## <a name="retiring-d15_v2ds15_v2-isolation-on-may-15-2020"></a>Retirada del aislamiento de D15_v2/DS15_v2 el 15 de mayo de 2020
**Actualización del 10 de febrero de 2020: La escala de tiempo de retirada del "aislamiento" se ha ampliado al 15 de mayo de 2020**

Azure Dedicated Host ya se encuentra con disponible con carácter general y permite ejecutar las máquinas virtuales Linux y Windows de la organización en servidores físicos de un solo inquilino. Tenemos previsto reemplazar por completo las máquinas virtuales de Azure aisladas con Azure Dedicated Host. A partir del **15 de mayo de 2020**, las máquinas virtuales de Azure D15_v2/DS15_v2 ya no estarán aisladas del hardware.

## <a name="how-does-this-affect-me"></a>¿Cómo me afecta esto?
A partir del 15 de mayo de 2020, ya no se proporcionará ninguna garantía de aislamiento para las máquinas virtuales de Azure D15_v2/DS15_v2. 

## <a name="what-actions-should-i-take"></a>¿Qué medidas debo tomar?
Si no es necesario el aislamiento de hardware, no tiene que hacer nada. 

En caso de que lo sea, antes del 15 de mayo de 2020 necesitaría hacer lo siguiente:

•   [Migrar](https://azure.microsoft.com/blog/introducing-azure-dedicated-host) la carga de trabajo a Azure Dedicated Host.

•   [Solicitar acceso](https://aka.ms/D15iRequestAccess) a una máquina virtual de Azure D15i_v2 y DS15i_v2 para conseguir la misma relación precio-rendimiento Esta opción solo está disponible para el modelo de pago por uso y para los escenarios de instancia reservada de un año.    

•   [Migrar](https://azure.microsoft.com/blog/resize-virtual-machines/) la carga de trabajo a otra máquina virtual aislada de Azure 

Consulte a continuación para más información:

## <a name="timeline"></a>Escala de tiempo
| Date | Acción | 
| --- | --- |
| 18 de noviembre de 2019 | Disponibilidad de D/DS15i_v2 (pago por uso, instancia reservada de un año) |
| 14 de mayo de 2020  | Último día para comprar la instancia reservada de un año de D/DS15i_v2 | 
| 15 de mayo de 2020   | Se ha quitado la garantía de aislamiento de D/DS15_v2 | 
| 15 de mayo de 2021  | Retirada de D/DS15i_v2 (todos los clientes, excepto los que compraron una instancia reservada de tres años de D/DS15_v2 antes del 18 de noviembre de 2019)| 
| 17 de noviembre de 2022  | Retirada de D/DS15i_v2 cuando se realizan instancias reservadas de tres años (para los clientes que compraron instancias reservadas de tres años de D/DS15_v2 antes del 18 de noviembre de 2019) | 

## <a name="faq"></a>Preguntas más frecuentes
### <a name="q-is-the-size-dds15_v2-going-to-get-retired"></a>P: ¿Se va a retirar el tamaño D/DS15_v2?
**R.** : No, solo se va a retirar la característica de "aislamiento". Si no necesita aislamiento, no tiene que hacer nada.

### <a name="q-is-the-size-dds15i_v2-going-to-get-retired"></a>P: ¿Se va a retirar el tamaño D/DS15i_v2?
**R.** : Sí, el tamaño solo está disponible hasta el 15 de mayo de 2021. Los clientes que han adquirido RI de 3 años en D/DS15_v2 antes del 18 de noviembre de 2019 van a tener acceso a D/DS15i_v2 hasta el 17 de noviembre de 2022.

### <a name="q-why-am-i-not-seeing-the-new-dds15i_v2-sizes-in-the-portal"></a>P: ¿Por qué no veo los nuevos tamaños de D/DS15i_v2 en el portal?
**R.** : Si ahora es cliente de D/DS15_v2 y quiere usar los nuevos tamaños D/DS15i_v2, rellene este [formulario](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0FTPNXHdWpJlO27GE-bHitUMkZUWEFPNjFPNVgyMkhZS05FSzlPTzRIOS4u).

### <a name="q-why-i-am-not-seeing-any-quota-for-the-new-dds15i_v2-sizes"></a>P: ¿Por qué no veo ninguna cuota para los nuevos tamaños de D/DS15i_v2?
**R.** : Si ahora es cliente de D/DS15_v2 y quiere usar los nuevos tamaños D/DS15i_v2, rellene este [formulario](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0FTPNXHdWpJlO27GE-bHitUNU1XUkhZWkNXQUFMNEJWUk9VWkRRVUJPMy4u).

### <a name="q-when-are-the-other-isolated-sizes-going-to-retire"></a>P: ¿Cuándo se van a retirar los otros tamaños aislados?
**R.** : Se enviarán recordatorios 12 meses antes de la retirada oficial de los tamaños.

### <a name="q-is-there-a-downtime-when-my-vm-lands-on-a-non-isolated-hardware"></a>P: ¿Hay un tiempo de inactividad cuando la máquina virtual se encuentra en un hardware no aislado?
**R.** : Si no necesita aislamiento, no tiene que hacer nada ni va a experimentar ningún tiempo de inactividad.

### <a name="q-are-there-any-cost-changes-for-moving-to-a-non-isolated-virtual-machine"></a>P: ¿Hay algún cambio de costo por cambiar a una máquina virtual no aislada?
**R.** : No 

### <a name="q-i-already-purchased-1--or-3-year-reserved-instance-for-d15_v2-or-ds15_v2-how-will-the-discount-be-applied-to-my-vm-usage"></a>P: Ya he comprado una instancia reservada de 1 o 3 años para D15_v2 o Ds15_v2. ¿Cómo se aplicará el descuento al uso de mi máquina virtual?
**R.** : Las instancias reservadas adquiridas antes del 18 de noviembre de 2019 extenderán automáticamente la cobertura a la nueva serie de máquinas virtuales aisladas. 

| Instancia reservada |  Flexibilidad de tamaño de instancia | Elegibilidad de las ventajas |   
| --- | --- | --- |
|   D15_v2  |   Off     |   D15_v2 y D15i_v2 |    
|   D15_v2  |   Por  |   Las series D15_v2 y D15i_v2 recibirán las ventajas de la instancia reservada de máquina virtual. |    
|   D14_v2  |   Por  |   Las series D15_v2 y D15i_v2 recibirán las ventajas de la instancia reservada de máquina virtual. |    
 
Del mismo modo para la serie Dsv2.
 
### <a name="q-i-want-to-purchase-additional-reserved-instances-for-dv2-which-one-should-i-choose"></a>P: Deseo comprar instancias reservadas adicionales para Dv2. ¿Cuál debo elegir?
**R.** : Todas las instancias reservadas adquiridas después de 18 de noviembre de 2019 tienen el siguiente comportamiento. 

| Instancia reservada |  Flexibilidad de tamaño de instancia | Elegibilidad de las ventajas |   
| --- | --- | --- |
| D15_v2 |  Off |   Solo D15_v2  
| D15_v2 |  Por |    La serie D15_v2 recibirá el beneficio de la instancia reservada. El nuevo D15i_v2 no será válido para la ventaja de la instancia reservada desde este tipo de instancia reservada. | 
| D15i_v2 |     Off | Solo D15i_v2 |  
| D15i_v2 |     Por  | Solo D15i_v2 | 
 
No se puede usar la flexibilidad de tamaño de instancia para aplicar a otros tamaños como D2_v2, D4_v2 o D15_v2. Del mismo modo para la serie Dsv2.  
 
### <a name="q-can-i-buy-a-new-3-year-ri-for-d15i_v2-and-ds15i_v2"></a>P: ¿Puedo comprar una nueva instancia reservada de tres años para D15i_v2 y DS15i_v2?
**R.** : Desafortunadamente no, solo están disponibles las instancias reservadas de un año para la nueva compra.
 
### <a name="q-can-i-move-my-existing-d15_v2ds15_v2-reserve-instance-to-an-isolated-size-reserved-instance"></a>P: ¿Puedo transferir mi instancia reservada existente de D15_v2 o DS15_v2 a una instancia reservada de tamaño aislado?
**R.** : Esto no es necesario, ya que la ventaja se aplica a los tamaños aislados y no aislados. Sin embargo, Azure admitirá el cambio de las instancias reservadas existentes de D15_v2 o DS15_v2 a las de D15i_v2 o DS15i_v2. Para todas las demás instancias reservadas de Dv2 o Dsv2, use la instancia reservada existente o compre nuevas instancias reservadas para los tamaños aislados.

### <a name="q-im-an-azure-service-fabric-customer-relying-on-the-silver-or-gold-durability-tiers-does-this-change-impact-me"></a>P: Soy un cliente de Azure Service Fabric que se basa en los niveles de durabilidad Silver o Gold. ¿Me afectará este cambio?
**R.** : No. Las garantías proporcionadas por los [niveles de durabilidad](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) de Service Fabric seguirán funcionando incluso después de este cambio. Si necesita aislamiento de hardware físico por otras razones, es posible que tenga que realizar una de las acciones descritas anteriormente. 

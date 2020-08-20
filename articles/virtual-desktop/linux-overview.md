---
title: 'Compatibilidad de Linux con Windows Virtual Desktop: Azure'
description: Una breve descripción general de la compatibilidad de Linux con Windows Virtual Desktop.
author: Heidilohr
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: bd3cc6c5220e2e84cbbd30b29b8034f53c813f1e
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "88008702"
---
# <a name="linux-support"></a>Compatibilidad de Linux

Puede acceder a los recursos de Windows Virtual Desktop desde sus dispositivos Linux con los siguientes clientes compatibles, que proporcionan nuestros asociados de cliente ligero de Linux. Estamos trabajando con varios asociados para habilitar clientes de Windows Virtual Desktop compatibles en más dispositivos y sistemas operativos basados en Linux. Si quiere que agreguemos una compatibilidad con Windows Virtual Desktop en una plataforma Linux que no aparece aquí, háganoslo saber en nuestra [página UserVoice](https://remotedesktop.uservoice.com/forums/923035-remote-desktop-support-on-linux).

## <a name="connect-with-your-linux-device"></a>Conexión con el dispositivo Linux

Los siguientes asociados tienen clientes de Windows Virtual Desktop aprobados para dispositivos Linux.

|Asociado|Documentación de asociado|Soporte técnico del asociado|
|:------|:--------------------|:--------------|
|![Logotipo de IGEL](./media/partners/igel.png)|[Documentación del cliente de IGEL](https://www.igel.com/igel-solution-family/windows-virtual-desktop/)|[Soporte técnico de IGEL](https://www.igel.com/support/)|

## <a name="what-is-the-linux-sdk"></a>¿Qué es el SDK de Linux?

Los asociados de cliente ligero de Linux pueden usar las API del SDK de Linux para Windows Virtual Desktop con el fin de recuperar fuentes de recursos, conectarse a sesiones de escritorio o de aplicaciones remotas y usar muchos de los redireccionamientos que admiten los clientes de nuestra plataforma. El SDK es compatible con la mayoría de los sistemas operativos basados en Ubuntu 18.04 o versiones posteriores.

### <a name="feature-support"></a>Compatibilidad de características

El SDK admite varias conexiones a sesiones de escritorio y aplicaciones remotas. Se admiten los siguientes redireccionamientos:

| Redireccionamiento       | Compatible |
| :---------------- | :-------: |
| Teclado          | &#10004;  |
| Mouse             | &#10004;  |
| Entrada de audio          | &#10004;  |
| Salida de audio         | &#10004;  |
| Portapapeles (texto)  | &#10004;  |
| Portapapeles (imagen) | &#10004;  |
| Portapapeles (archivo)  | &#10004;  |
| Tarjeta inteligente         | &#10004;  |
| Unidad o carpeta      | &#10004;  |

El SDK también admite configuraciones de varios monitores, siempre y cuando seleccione monitores contiguos para su sesión.

Este documento se actualizará a medida que se agregue compatibilidad con nuevas características y redireccionamientos. Si quiere sugerir nuevas características y otras mejoras, visite nuestra [página de UserVoice](https://go.microsoft.com/fwlink/?linkid=2116523).

## <a name="next-steps"></a>Pasos siguientes

Consulte la documentación de los siguientes clientes:

- [Cliente de escritorio de Windows](connect-windows-7-10.md)
- [Cliente web](connect-web.md)
- [Cliente de Android](connect-android.md)
- [Cliente de macOS](connect-macos.md)
- [Cliente de iOS](connect-ios.md)

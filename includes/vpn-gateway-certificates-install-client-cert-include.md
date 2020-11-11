---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/29/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 393b29245141b2970e7c1a227d6e8b1b131c445c
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93061659"
---
Si desea crear una conexión P2S desde un equipo cliente distinto del que usó para generar los certificados de cliente, debe instalar un certificado de cliente. Al instalar un certificado de cliente, necesita la contraseña que se creó cuando se exportó el certificado de cliente.

1. Busque el archivo *.pfx* y cópielo en el equipo cliente. En el equipo cliente, haga doble clic en el archivo *.pfx* para instalarlo. Deje **Ubicación del almacén** como **Usuario actual** y seleccione **Siguiente**.
1. En la página **File to import** (Archivo para importar), no haga ningún cambio. Seleccione **Siguiente**.
1. En la página **Protección de clave privada** , escriba la contraseña del certificado o compruebe que la entidad de seguridad sea correcta y seleccione **Siguiente**.
1. En la página **Almacén de certificados** , deje la ubicación predeterminada y seleccione **Siguiente**.
1. Seleccione **Finalizar**. En la **advertencia de seguridad** para la instalación de certificados, seleccione **Sí**. Puede seleccionar con total tranquilidad el valor "Sí" en esta advertencia de seguridad, ya que ha generado el certificado.
1. El certificado se importó correctamente.

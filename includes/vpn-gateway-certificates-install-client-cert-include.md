---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 0068bd151c3d7d243b05c326ec73a201f4131296
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "67186207"
---
Si desea crear una conexión P2S desde un equipo cliente distinto del que usó para generar los certificados de cliente, debe instalar un certificado de cliente. Al instalar un certificado de cliente, necesita la contraseña que se creó cuando se exportó el certificado de cliente.

1. Busque el archivo *.pfx* y cópielo en el equipo cliente. En el equipo cliente, haga doble clic en el archivo *.pfx* para instalarlo. Deje **Ubicación del almacén** como **Usuario actual** y haga clic en **Siguiente**.
2. En la página **File to import** (Archivo para importar), no haga ningún cambio. Haga clic en **Next**.
3. En la página **Protección de clave privada**, escriba la contraseña del certificado o compruebe que la entidad de seguridad sea correcta y haga clic en **Siguiente**.
4. En la página **Almacén de certificados**, deje la ubicación predeterminada y, luego, haga clic en **Siguiente**.
5. Haga clic en **Finalizar** En la **Advertencia de seguridad** para la instalación de certificados, haga clic en **Sí**. Al haberse generado el certificado, puede hacer clic en "Sí" de forma segura. El certificado se importó correctamente.
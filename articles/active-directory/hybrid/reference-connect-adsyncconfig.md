---
title: 'Azure AD Connect: Referencia de PowerShell AdSyncConfig | Microsoft Docs'
description: En este documento se proporciona información de referencia para el módulo de PowerShell ADSyncConfig.psm1.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.date: 01/24/2019
ms.subservice: hybrid
ms.author: billmath
ms.topic: reference
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8159ef45dee8a2f9ace69c2a5b66a29e4948d82c
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82982010"
---
# <a name="azure-ad-connect--adsyncconfig-powershell-reference"></a>Azure AD Connect:  Referencia de PowerShell ADSyncConfig
En la siguiente documentación se proporciona información de referencia para el módulo de PowerShell ADSyncConfig.psm1 que se incluye con Azure AD Connect.


## <a name="get-adsyncadconnectoraccount"></a>Get-ADSyncADConnectorAccount

### <a name="synopsis"></a>SINOPSIS
Obtiene el nombre y el dominio de la cuenta que se configura en cada Conector AD

### <a name="syntax"></a>SINTAXIS

```
Get-ADSyncADConnectorAccount
```

### <a name="description"></a>DESCRIPTION
Esta función usa el cmdlet "Get-ADSyncConnector" que está presente en AAD Connect para recuperar de Parámetros de conectividad una tabla que muestra la cuenta de Conectores AD.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Get-ADSyncADConnectorAccount
```

## <a name="get-adsyncobjectswithinheritancedisabled"></a>Get-ADSyncObjectsWithInheritanceDisabled

### <a name="synopsis"></a>SINOPSIS
Obtiene objetos de AD con la herencia de permisos deshabilitada

### <a name="syntax"></a>SINTAXIS

```
Get-ADSyncObjectsWithInheritanceDisabled [-SearchBase] <String> [[-ObjectClass] <String>] [<CommonParameters>]
```

### <a name="description"></a>DESCRIPTION
Busca en AD a partir del parámetro SearchBase y devuelve todos los objetos, filtrados por el parámetro ObjectClass, que tienen la herencia de ACL deshabilitada actualmente.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
Busca objetos con la herencia deshabilitada en el dominio "contoso" (de forma predeterminada, solo devuelve objetos "organizationalUnit").
```
Get-ADSyncObjectsWithInheritanceDisabled -SearchBase 'Contoso'
```

#### <a name="example-2"></a>EJEMPLO 2
Busca objetos "user" con la herencia deshabilitada en el dominio "contoso".
```
Get-ADSyncObjectsWithInheritanceDisabled -SearchBase 'Contoso' -ObjectClass 'user'
```

#### <a name="example-3"></a>EJEMPLO 3
Busca todos los tipos de objetos con la herencia deshabilitada en una unidad organizativa.
```
Get-ADSyncObjectsWithInheritanceDisabled -SearchBase OU=AzureAD,DC=Contoso,DC=com -ObjectClass '*'
```



### <a name="parameters"></a>PARAMETERS

#### <a name="-searchbase"></a>-SearchBase
SearchBase para la consulta LDAP que puede ser DistinguishedName del dominio de AD o un FQDN

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-objectclass"></a>-ObjectClass
Clase de los objetos que se van a buscar que puede ser "*" (para cualquier clase de objeto), "user", "group", "container", etc. De manera predeterminada, esta función buscará la clase de objeto "organizationalUnit".

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: OrganizationalUnit
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncbasicreadpermissions"></a>Set-ADSyncBasicReadPermissions

### <a name="synopsis"></a>SINOPSIS
Inicialice el bosque y el dominio de Active Directory para los permisos de lectura básicos.

### <a name="syntax"></a>SINTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [-SkipAdminSdHolders]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>DESCRIPTION
La función Set-ADSyncBasicReadPermissions proporcionará los permisos necesarios para la cuenta de sincronización de AD, que incluyen los siguientes:
1.
Acceso a la propiedad de lectura en todos los atributos para todos los objetos computer descendientes
2.
Acceso a la propiedad de lectura en todos los atributos para todos los objetos device descendientes
3.
Acceso a la propiedad de lectura en todos los atributos para todos los objetos foreignSecurityPrincipal descendientes
5.
Acceso a la propiedad de lectura en todos los atributos para todos los objetos user descendientes
6.
Acceso a la propiedad de lectura en todos los atributos para todos los objetos inetorgperson descendientes
7.
Acceso a la propiedad de lectura en todos los atributos para todos los objetos group descendientes
8.
Acceso a la propiedad de lectura en todos los atributos para todos los objetos contact descendientes

Estos permisos se aplican a todos los dominios del bosque.
De manera opcional, puede proporcionar DistinguishedName en el parámetro ADobjectDN para establecer estos permisos en ese objeto de AD únicamente (incluida la herencia a los objetos secundarios).

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>EJEMPLO 2
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>EJEMPLO 3
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>EJEMPLO 4
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Name de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Domain de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
DistinguishedName del objeto AD de destino para establecer permisos (opcional)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
Parámetro opcional para indicar si el contenedor AdminSDHolder no se debe actualizar con estos permisos

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Muestra lo que sucedería si se ejecutara el cmdlet.
El cmdlet no se ejecuta.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Confirm
Le solicita su confirmación antes de ejecutar el cmdlet.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncexchangehybridpermissions"></a>Set-ADSyncExchangeHybridPermissions

### <a name="synopsis"></a>SINOPSIS
Inicialice el bosque y el dominio de Active Directory para la característica Exchange Hybrid.

### <a name="syntax"></a>SINTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [-SkipAdminSdHolders]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>DESCRIPTION
La función Set-ADSyncExchangeHybridPermissions proporcionará los permisos necesarios para la cuenta de sincronización de AD, que incluyen los siguientes:
1.
Acceso a la propiedad de lectura/escritura en todos los atributos para todos los objetos user descendientes
2.
Acceso a la propiedad de lectura/escritura en todos los atributos para todos los objetos inetorgperson descendientes
3.
Acceso a la propiedad de lectura/escritura en todos los atributos para todos los objetos group descendientes
4.
Acceso a la propiedad de lectura/escritura en todos los atributos para todos los objetos contact descendientes

Estos permisos se aplican a todos los dominios del bosque.
De manera opcional, puede proporcionar DistinguishedName en el parámetro ADobjectDN para establecer estos permisos en ese objeto de AD únicamente (incluida la herencia a los objetos secundarios).

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>EJEMPLO 2
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>EJEMPLO 3
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>EJEMPLO 4
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Name de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Domain de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
DistinguishedName del objeto AD de destino para establecer permisos (opcional)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
Parámetro opcional para indicar si el contenedor AdminSDHolder no se debe actualizar con estos permisos

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Muestra lo que sucedería si se ejecutara el cmdlet.
El cmdlet no se ejecuta.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Confirm
Le solicita su confirmación antes de ejecutar el cmdlet.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncexchangemailpublicfolderpermissions"></a>Set-ADSyncExchangeMailPublicFolderPermissions

### <a name="synopsis"></a>SINOPSIS
Inicialice el bosque y el dominio de Active Directory para la característica Carpeta pública de correo de Exchange.

### <a name="syntax"></a>SINTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName <String>
 -ADConnectorAccountDomain <String> [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>DESCRIPTION
La función Set-ADSyncExchangeMailPublicFolderPermissions proporcionará los permisos necesarios para la cuenta de sincronización de AD, que incluyen los siguientes:
1.
Acceso a la propiedad de lectura en todos los atributos para todos los objetos publicfolder descendientes

Estos permisos se aplican a todos los dominios del bosque.
De manera opcional, puede proporcionar DistinguishedName en el parámetro ADobjectDN para establecer estos permisos en ese objeto de AD únicamente (incluida la herencia a los objetos secundarios).

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>EJEMPLO 2
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>EJEMPLO 3
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>EJEMPLO 4
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Name de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Domain de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
DistinguishedName del objeto AD de destino para establecer permisos (opcional)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
Parámetro opcional para indicar si el contenedor AdminSDHolder no se debe actualizar con estos permisos

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Muestra lo que sucedería si se ejecutara el cmdlet.
El cmdlet no se ejecuta.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Confirm
Le solicita su confirmación antes de ejecutar el cmdlet.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncmsdsconsistencyguidpermissions"></a>Set-ADSyncMsDsConsistencyGuidPermissions

### <a name="synopsis"></a>SINOPSIS
Inicialice el bosque y el dominio de Active Directory para la característica mS-DS-ConsistencyGuid.

### <a name="syntax"></a>SINTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>DESCRIPTION
La función Set-ADSyncMsDsConsistencyGuidPermissions proporcionará los permisos necesarios para la cuenta de sincronización de AD, que incluyen los siguientes:
1.
Acceso a la propiedad de lectura/escritura en el atributo mS-DS-ConsistencyGuid para todos los objetos user descendientes

Estos permisos se aplican a todos los dominios del bosque.
De manera opcional, puede proporcionar DistinguishedName en el parámetro ADobjectDN para establecer estos permisos en ese objeto de AD únicamente (incluida la herencia a los objetos secundarios).

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>EJEMPLO 2
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>EJEMPLO 3
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>EJEMPLO 4
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Name de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Domain de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
DistinguishedName del objeto AD de destino para establecer permisos (opcional)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
Parámetro opcional para indicar si el contenedor AdminSDHolder no se debe actualizar con estos permisos

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Muestra lo que sucedería si se ejecutara el cmdlet.
El cmdlet no se ejecuta.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Confirm
Le solicita su confirmación antes de ejecutar el cmdlet.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncpasswordhashsyncpermissions"></a>Set-ADSyncPasswordHashSyncPermissions

### <a name="synopsis"></a>SINOPSIS
Inicialice el bosque y el dominio de Active Directory para la sincronización de hash de contraseña.

### <a name="syntax"></a>SINTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN <String> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>DESCRIPTION
La función Set-ADSyncPasswordHashSyncPermissions proporcionará los permisos necesarios para la cuenta de sincronización de AD, que incluyen los siguientes:
1.
Replicación de los cambios de directorio
2.
Replicación de todos los cambios de directorio

Estos permisos se conceden a todos los dominios del bosque.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>EJEMPLO 2
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Name de la cuenta de Active Directory que usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Domain de la cuenta de Active Directory que usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName de la cuenta de Active Directory que usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Muestra lo que sucedería si se ejecutara el cmdlet.
El cmdlet no se ejecuta.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Confirm
Le solicita su confirmación antes de ejecutar el cmdlet.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncpasswordwritebackpermissions"></a>Set-ADSyncPasswordWritebackPermissions

### <a name="synopsis"></a>SINOPSIS
Inicialice el bosque y el dominio de Active Directory para la escritura diferida de contraseñas desde Azure AD.

### <a name="syntax"></a>SINTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>DESCRIPTION
La función Set-ADSyncPasswordWritebackPermissions proporcionará los permisos necesarios para la cuenta de sincronización de AD, que incluyen los siguientes:
1.
Restablecimiento de contraseña en los objetos user descendientes
2.
Acceso a la propiedad de lectura en el atributo lockoutTime para todos los objetos user descendientes
3.
Acceso a la propiedad de lectura en el atributo pwdLastSet para todos los objetos user descendientes

Estos permisos se aplican a todos los dominios del bosque.
De manera opcional, puede proporcionar DistinguishedName en el parámetro ADobjectDN para establecer estos permisos en ese objeto de AD únicamente (incluida la herencia a los objetos secundarios).

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>EJEMPLO 2
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>EJEMPLO 3
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>EJEMPLO 4
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Name de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Domain de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
DistinguishedName del objeto AD de destino para establecer permisos (opcional)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
Parámetro opcional para indicar si el contenedor AdminSDHolder no se debe actualizar con estos permisos

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Muestra lo que sucedería si se ejecutara el cmdlet.
El cmdlet no se ejecuta.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Confirm
Le solicita su confirmación antes de ejecutar el cmdlet.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncrestrictedpermissions"></a>Set-ADSyncRestrictedPermissions

### <a name="synopsis"></a>SINOPSIS
Refuerce los permisos en un objeto de AD que, en caso contrario, no se incluya en ningún grupo de seguridad protegido de AD.
Un ejemplo típico es la cuenta de AD Connect (MSOL) que AAD Connect crea automáticamente.
Esta cuenta tiene permisos de replicación en todos los dominios, pero puede verse en riesgo fácilmente, ya que no está protegida.

### <a name="syntax"></a>SINTAXIS

```
Set-ADSyncRestrictedPermissions [-ADConnectorAccountDN] <String> [-Credential] <PSCredential>
 [-DisableCredentialValidation] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>DESCRIPTION
La función Set-ADSyncRestrictedPermissions reforzará los permisos para la cuenta proporcionada.
Para limitar los permisos, es necesario realizar los pasos siguientes:
1.
Deshabilite la herencia en el objeto especificado.
2.
Quite todas las ACE del objeto específico, excepto las ACE específicas de SELF.
Deseamos mantener intactos los permisos predeterminados cuando se trata de SELF.
3.
Asigne estos permisos específicos:

        Type    Name                                        Access              Applies To
        =============================================================================================
        Allow   SYSTEM                                      Full Control        This object
        Allow   Enterprise Admins                           Full Control        This object
        Allow   Domain Admins                               Full Control        This object
        Allow   Administrators                              Full Control        This object

        Allow   Enterprise Domain Controllers               List Contents
                                                            Read All Properties
                                                            Read Permissions    This object

        Allow   Authenticated Users                         List Contents
                                                            Read All Properties
                                                            Read Permissions    This object

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Set-ADSyncRestrictedPermissions -ADConnectorAccountDN "CN=TestAccount1,CN=Users,DC=Contoso,DC=com" -Credential $(Get-Credential)
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName de la cuenta de Active Directory cuyos permisos se deben reforzar.
Esta suele ser la cuenta MSOL_nnnnnnnnnn o una cuenta de dominio personalizada que está configurada en el Conector AD.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-credential"></a>-Credential
Credencial de administrador que tiene los privilegios necesarios para restringir los permisos de la cuenta ADConnectorAccountDN. Por lo general, suele ser el administrador de organización o de dominio. Use el nombre de dominio completo de la cuenta de administrador para evitar errores de búsqueda de cuenta.
Ejemplo: CONTOSO\admin

```yaml
Type: PSCredential
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-disablecredentialvalidation"></a>-DisableCredentialValidation
Cuando se usa DisableCredentialValidation, la función no comprobará si las credenciales proporcionadas en -Credential son válidos en AD y si la cuenta proporcionada tiene los privilegios necesarios para restringir los permisos de la cuenta ADConnectorAccountDN.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Muestra lo que sucedería si se ejecutara el cmdlet.
El cmdlet no se ejecuta.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Confirm
Le solicita su confirmación antes de ejecutar el cmdlet.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncunifiedgroupwritebackpermissions"></a>Set-ADSyncUnifiedGroupWritebackPermissions

### <a name="synopsis"></a>SINOPSIS
Inicialice el bosque y el dominio de Active Directory para la escritura diferida de grupos desde Azure AD.

### <a name="syntax"></a>SINTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>DESCRIPTION
La función Set-ADSyncUnifiedGroupWritebackPermissions proporcionará los permisos necesarios para la cuenta de sincronización de AD, que incluyen los siguientes:
1.
Lectura/escritura genéricas, eliminación, eliminación del árbol y creación\eliminación de elementos secundarios para todos los tipos Object y SubObjects del grupo

Estos permisos se aplican a todos los dominios del bosque.
De manera opcional, puede proporcionar DistinguishedName en el parámetro ADobjectDN para establecer estos permisos en ese objeto de AD únicamente (incluida la herencia a los objetos secundarios).
En este caso, ADobjectDN será DistinguishedName del contenedor que quiere vincular con la característica GroupWriteback.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>EJEMPLO 2
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>EJEMPLO 3
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>EJEMPLO 4
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Name de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Domain de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName de la cuenta de Active Directory que usa o usará Azure AD Connect Sync para administrar objetos en el directorio.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
DistinguishedName del objeto AD de destino para establecer permisos (opcional)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
Parámetro opcional para indicar si el contenedor AdminSDHolder no se debe actualizar con estos permisos

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Muestra lo que sucedería si se ejecutara el cmdlet.
El cmdlet no se ejecuta.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Confirm
Le solicita su confirmación antes de ejecutar el cmdlet.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="show-adsyncadobjectpermissions"></a>Show-ADSyncADObjectPermissions

### <a name="synopsis"></a>SINOPSIS
Muestra los permisos de un objeto de AD especificado.

### <a name="syntax"></a>SINTAXIS

```
Show-ADSyncADObjectPermissions [-ADobjectDN] <String> [<CommonParameters>]
```

### <a name="description"></a>DESCRIPTION
Esta función devuelve todos los permisos de AD actualmente establecidos para un objeto de AD dado que se proporciona en el parámetro -ADobjectDN.
ADobjectDN debe proporcionarse en un formato DistinguishedName.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Show-ADSyncADObjectPermissions -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adobjectdn"></a>-ADobjectDN
{{Indicar descripción de ADobjectDN}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

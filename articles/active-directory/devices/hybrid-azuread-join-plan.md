---
title: Planeación de la implementación de Unión a Azure Active Directory híbrido en Azure Active Directory (Azure AD) | Microsoft Docs
description: Aprenda a configurar dispositivos híbridos unidos a Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 306382a7dede44a0f1db53373e14e81cb54098ca
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70914733"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Instrucciones: Planeamiento de la implementación de la unión a Azure Active Directory híbrido

De forma similar a un usuario, un dispositivo es otra identidad principal que debe proteger y usarla también para proteger los recursos en cualquier momento y lugar. Para lograr este objetivo, puede llevar las identidades de los dispositivos a Azure AD y administrarlas ahí mediante uno de los métodos siguientes:

- Unión a Azure AD
- Unión a Azure AD híbrido
- Registro de Azure AD

Al traer sus dispositivos a Azure AD, está maximizando la productividad de los usuarios mediante un inicio de sesión único (SSO) en los recursos de nube y del entorno local. Al mismo tiempo, puede proteger el acceso a los recursos de nube y de entorno local con [acceso condicional](../active-directory-conditional-access-azure-portal.md).

Si tiene un entorno local de Active Directory (AD) y quiere unir sus equipos unidos a un dominio AD a Azure AD, puede realizar una unión a Azure AD híbrido. En este artículo se proporcionan los pasos relacionados con la implementación de una unión a Azure AD híbrido en su entorno. 

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por hecho que está familiarizado con la [introducción a la administración de identidades de dispositivos en Azure Active Directory](../device-management-introduction.md).

> [!NOTE]
> La versión del controlador de dominio mínima necesaria para la unión de dispositivos Windows 10 a Azure AD híbrido es Windows Server 2008 R2.

## <a name="plan-your-implementation"></a>Planeamiento de la implementación

Para planear la implementación de Azure AD híbrido, debe familiarizarse con:

|   |   |
| --- | --- |
| ![Comprobar][1] | Revisión de los dispositivos compatibles |
| ![Comprobar][1] | Revisión de los aspectos que debe conocer |
| ![Comprobar][1] | Revisión de la validación controlada de la unión a Azure AD híbrido |
| ![Comprobar][1] | Seleccione el escenario según la infraestructura de identidad |
| ![Comprobar][1] | Revisión del UPN de AD local para la unión a Azure AD híbrido |

## <a name="review-supported-devices"></a>Revisión de los dispositivos compatibles

La unión a Azure AD híbrido admite una amplia variedad de dispositivos Windows. Dado que la configuración para los dispositivos que ejecutan versiones anteriores de Windows requiere pasos adicionales o diferentes, los dispositivos compatibles se agrupan en dos categorías:

### <a name="windows-current-devices"></a>Dispositivos de Windows actuales

- Windows 10
- Windows Server 2016
- Windows Server 2019

Para dispositivos que ejecutan el sistema operativo de escritorio Windows, las versiones admitidas se enumeran en el artículo [Información sobre la versión de Windows 10](https://docs.microsoft.com/windows/release-information/). Como procedimiento recomendado, Microsoft recomienda actualizar a la versión más reciente de Windows 10.

### <a name="windows-down-level-devices"></a>Dispositivos de Windows de nivel inferior

- Windows 8.1
- Windows 7. Para obtener información de soporte técnico sobre Windows 7, consulte [El soporte técnico para Windows 7 está llegando a su fin](https://www.microsoft.com/microsoft-365/windows/end-of-windows-7-support).
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2. Para obtener información de soporte técnico sobre Windows Server 2008 y 2008 R2, consulte [Preparación para la finalización del soporte técnico de Windows Server 2008](https://www.microsoft.com/cloud-platform/windows-server-2008).

Como primer paso del planeamiento, debe revisar el entorno y determinar si necesita admitir dispositivos Windows de nivel inferior.

## <a name="review-things-you-should-know"></a>Revisión de los aspectos que debe conocer

Actualmente, no se admite la unión a Azure AD híbrido si el entorno consta de un solo bosque AD que sincroniza datos de identidad con más de un inquilino de Azure AD.

Actualmente, no se admite la unión a Azure AD híbrido cuando se usa la infraestructura de escritorio virtual (VDI).

La unión a Azure AD híbrido es compatible con TPM 2.0 compatible con FIPS y no se admite en TPM 1.2. Si los dispositivos tienen TPM 1.2 compatible con FIPS, debe deshabilitarlos antes de continuar con la unión a Azure AD híbrido. Microsoft no proporciona ninguna herramienta para deshabilitar el modo FIPS para TPM, ya que eso depende del fabricante de TPM. Póngase en contacto con el OEM de hardware para obtener soporte técnico. A partir de la versión 10 1903 de Windows, los TPM 1,2 no se usan para la unión de Azure AD híbrida y los dispositivos que tengan esos TPM se considerarán como si no tuvieran un TPM.

No se admite la unión a Azure AD híbrido para Windows Server que ejecuta el rol de controlador de dominio (DC).

No se admite la unión a Azure AD híbrido en dispositivos Windows de nivel inferior al usar la movilidad de credenciales o de perfiles de usuario.

Si se basa en la herramienta de preparación del sistema (Sysprep) y utiliza para la instalación una imagen **anterior a Windows 10 1809**, asegúrese de que esa imagen no corresponde a un dispositivo que ya está registrado en Azure AD como unión a Azure AD híbrido.

Si se basa en la instantánea de una máquina virtual (VM) para crear otras VM, asegúrese de que esa instantánea no sea de una VM que ya se haya registrado en Azure AD como unión a Azure AD híbrido.

Si los dispositivos unidos a un dominio de Windows 10 están [registrados en Azure AD](overview.md#getting-devices-in-azure-ad) con su inquilino, podría provocar un doble estado de dispositivos registrados en Azure AD y unidos a Azure AD híbrido. Se recomienda actualizar a Windows 10 1803 (con KB4489894 aplicado) o una versión superior para solucionar automáticamente esta situación. En las versiones anteriores a 1803, deberá quitar manualmente el estado registrado en Azure AD antes de habilitar la unión a Azure AD híbrido. A partir de la versión 1803, se han realizado los siguientes cambios para evitar este doble estado:

- Cualquier estado registrado en Azure AD existente se eliminará automáticamente <i>después de que el dispositivo se una a Azure AD híbrido</i>.
- Puede impedir que el dispositivo unido al dominio se registre en Azure AD mediante la incorporación de esta clave del registro: HKLM\SOFTWARE\Policies\Microsoft\Windows\WorkplaceJoin, "BlockAADWorkplaceJoin"=dword:00000001.
- En Windows 10 1803, si tiene configurado Windows Hello para empresas, el usuario tendrá que volver a configurarlo tras la limpieza del doble estado. Este problema se ha resuelto con KB4512509.



## <a name="review-controlled-validation-of-hybrid-azure-ad-join"></a>Revisión de la validación controlada de la unión a Azure AD híbrido

Una vez cumplidos todos los requisitos previos, los dispositivos Windows se registrarán automáticamente como dispositivos en el inquilino de Azure AD. El estado de estas identidades de dispositivo en Azure AD se conoce como unión a Azure AD híbrido. Puede obtener más información sobre los conceptos tratados en este artículo en los artículos [Introducción a la administración de identidades de dispositivo en Azure Active Directory](overview.md) y [Planear la implementación de la unión a Azure Active Directory híbrido](hybrid-azuread-join-plan.md).

Es posible las organizaciones estén interesadas en realizar una validación controlada de la unión a Azure AD híbrido antes de habilitarla en toda la organización a la vez. Consulte el artículo [Validación controlada de la unión a Azure AD híbrido](hybrid-azuread-join-control.md) para entender cómo llevarla a cabo.

## <a name="select-your-scenario-based-on-your-identity-infrastructure"></a>Seleccione el escenario según la infraestructura de identidad

La unión a Azure AD híbrido funciona con entornos administrados y federados, en función de si la UPN es enrutable o no enrutable. En la parte inferior de la página puede consultar una tabla sobre los escenarios admitidos.  

### <a name="managed-environment"></a>Entorno administrado

Un entorno administrado se puede implementar mediante [Sincronización de hash de contraseña (PHS)](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-phs) o [Autenticación de paso a través (PTA)](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta) con [Inicio de sesión único de conexión directa](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso).

Estos escenarios no requieren que se configure un servidor de federación para la autenticación.

### <a name="federated-environment"></a>Entorno federado

Un entorno federado debe tener un proveedor de identidades que admita los requisitos siguientes. Si tiene un entorno federado en que se utilizan los Servicios de federación de Active Directory (AD FS), ya se admiten los requisitos anteriores.

- **Notificación WIAORMULTIAUTHN:** Esta notificación es necesaria para realizar la unión a Azure AD híbrido para dispositivos Windows de nivel inferior.
- **Protocolo WS-Trust:** Este protocolo es necesario para autenticar en Azure AD los dispositivos Windows actuales unidos a Azure AD híbrido. Cuando use AD FS, debe habilitar los siguientes puntos de conexión de WS-Trust: `/adfs/services/trust/2005/windowstransport`  
`/adfs/services/trust/13/windowstransport`  
  `/adfs/services/trust/2005/usernamemixed` 
  `/adfs/services/trust/13/usernamemixed`
  `/adfs/services/trust/2005/certificatemixed` 
  `/adfs/services/trust/13/certificatemixed` 

> [!WARNING] 
> Tanto **adfs/services/trust/2005/windowstransport** como **adfs/services/trust/13/windowstransport** se deben habilitar como puntos de conexión accesibles desde la intranet y NO deben exponerse como accesible desde Proxy de aplicación web. Para más información sobre cómo deshabilitar los puntos de conexión de Windows de WS-Trust, consulte [Deshabilitar los puntos de conexión de Windows de WS-Trust en el proxy](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/best-practices-securing-ad-fs#disable-ws-trust-windows-endpoints-on-the-proxy-ie-from-extranet). Para ver qué puntos de conexión están habilitados, vaya a **Servicio** > **Puntos de conexión** en la consola de administración de AD FS.

> [!NOTE]
> Azure AD no admite tarjetas inteligentes ni certificados en dominios administrados.

Desde la versión 1.1.819.0, Azure AD Connect proporciona un asistente para configurar la unión a Azure AD híbrido. El asistente permite simplificar considerablemente el proceso de configuración. Si no le es posible instalar la versión requerida de Azure AD Connect, consulte la información sobre [cómo configurar manualmente el registro de dispositivos](hybrid-azuread-join-manual.md). 

Según el escenario que coincida con la infraestructura de identidad, consulte los temas siguientes:

- [Configuración de la unión a Azure Active Directory híbrido para un entorno federado](hybrid-azuread-join-federated-domains.md)
- [Configuración de la unión a Azure Active Directory híbrido para un entorno administrado](hybrid-azuread-join-managed-domains.md)

## <a name="review-on-premises-ad-upn-support-for-hybrid-azure-ad-join"></a>Revisión del soporte de UPN de AD local para la unión a Azure AD híbrido

A veces, los UPN de AD local podrían ser diferentes de los UPN de Azure AD. En tales casos, Unión a Azure AD híbrido para Windows 10 proporciona compatibilidad limitada para los UPN de AD local, según el tipo de [método de autenticación](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn), el tipo de dominio y la versión de Windows 10. Hay dos tipos de UPN de AD local que pueden existir en su entorno:

- UPN enrutable: Un UPN enrutable tiene un dominio verificado válido, que está registrado en un registrador de dominios. Por ejemplo, si contoso.com es el dominio principal, en Azure AD, contoso.org es el dominio principal en la instancia local de AD que pertenece a Contoso y que está [verificado en Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain)
- UPN no enrutable: Un UPN no enrutable no tiene un dominio verificado. Es aplicable solo dentro de la red privada de su organización. Por ejemplo, si contoso.com es el dominio principal en Azure AD, contoso.local es el dominio principal en la instancia local de AD, pero no es un dominio verificable en Internet, y solo se usa dentro de la red de Contoso.

En la tabla siguiente se proporcionan detalles sobre la compatibilidad de estos UPN de AD local en Unión a Azure AD híbrido para Windows 10.

| Tipo de UPN de AD local | Tipo de dominio | Versión de Windows 10 | DESCRIPCIÓN |
| ----- | ----- | ----- | ----- |
| Enrutable | Federado | A partir de la versión 1703 | Disponibilidad general |
| No enrutable | Federado | A partir de la versión 1803 | Disponibilidad general |
| Enrutable | Administrado | A partir de la versión 1803 | Disponible con carácter general, no se admite el servicio SSPR de Azure AD en la pantalla de bloqueo de Windows |
| No enrutable | Administrado | No compatible | |

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración de la unión a Azure Active Directory híbrido para un entorno federado](hybrid-azuread-join-federated-domains.md)
> [Configuración de la unión a Azure Active Directory híbrido para un entorno administrado](hybrid-azuread-join-managed-domains.md)

<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png

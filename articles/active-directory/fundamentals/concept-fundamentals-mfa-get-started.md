---
title: Aktivieren der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für Ihre Organisation – Azure Active Directory
description: Aktivieren von Azure MFA für Ihre Organisation basierend auf Ihrer Lizenz
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe0d79f228ee2b84c00a35d4836cbda6a4847a42
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74932175"
---
# <a name="enable-multi-factor-authentication-for-your-organization"></a>Aktivieren der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für Ihre Organisation

Es gibt mehrere Möglichkeiten, Azure Multi-Factor Authentication (MFA) für Ihre Benutzer von Azure Active Directory (Azure AD) basierend auf den Lizenzen zu aktivieren, die Ihre Organisation besitzt. 

![Signale untersuchen und bei Bedarf MFA erzwingen](./media/concept-fundamentals-mfa-get-started/verify-signals-and-perform-mfa-if-required.png)

Basierend auf unseren Studien ist Ihr Konto bei Verwendung von MFA mehr als 99,9 % weniger wahrscheinlich gefährdet.

Wie kann Ihre Organisation also die mehrstufige Authentifizierung auch kostenlos aktivieren, bevor sie zu einer Statistik wird?

## <a name="free-option"></a>Kostenlose Option

Kunden, die die kostenlosen Vorteile von Azure AD nutzen, können mithilfe von [Sicherheitsstandards](../fundamentals/concept-fundamentals-security-defaults.md) die mehrstufige Authentifizierung in ihrer Umgebung aktivieren.

## <a name="office-365"></a>Office 365

Kunden, die über Office 365 verfügen, stehen zwei Optionen zur Verfügung:

- [Sicherheitsstandards](concept-fundamentals-security-defaults.md) können über Azure AD aktiviert werden, um alle Benutzer mit Azure Multi-Factor Authentication zu schützen.
- Wenn Ihre Organisation bei der Bereitstellung von Multi-Factor Authentication mehr Granularität benötigt, bieten Ihre Office-Lizenzen [benutzerbasierte MFA](../authentication/howto-mfa-userstates.md)-Funktionen. Benutzerbasierte MFA wird von Administratoren für jeden Benutzer einzeln aktiviert und erzwungen.

## <a name="azure-ad-premium-p1"></a>Azure AD Premium P1

Für Kunden, die über Azure AD Premium P1- oder ähnliche Lizenzen mit dieser Funktionalität verfügen (z. B. Enterprise Mobility + Security E3, Microsoft 365 F1 oder Microsoft 365 E3), gilt Folgendes: 

Es wird empfohlen, [Richtlinien für bedingten Zugriff](../conditional-access/concept-conditional-access-policy-common.md) zu verwenden, um ein optimales Benutzererlebnis zu ermöglichen.

## <a name="azure-ad-premium-p2"></a>Azure AD Premium P2

Für Kunden, die über Azure AD Premium P2- oder ähnliche Lizenzen mit dieser Funktionalität verfügen (z. B. Enterprise Mobility + Security E5 oder Microsoft 365 E5), gilt Folgendes: 

Es wird empfohlen, [Richtlinien für bedingten Zugriff](../conditional-access/concept-conditional-access-policy-common.md) zusammen mit [Identity Protection](../identity-protection/overview-v2.md)-Risikorichtlinien zu verwenden, um ein optimales Benutzererlebnis und Flexibilität bei der Erzwingung zu ermöglichen.

## <a name="authentication-methods"></a>Authentifizierungsmethoden

|   | Standardwerte für die Sicherheit | Alle anderen Methoden |
| --- | --- | --- |
| Benachrichtigung über mobile App | X | X |
| Prüfcode aus mobiler App oder Hardwaretoken |   | X |
| Textnachricht an Telefon |   | X |
| Auf Telefon anrufen |   | X |
| App-Kennwörter |   | X** |

** App-Kennwörter sind nur in Szenarien mit benutzerbasierter MFA mit Legacyauthentifizierung verfügbar, wenn diese Methode von Administratoren aktiviert wurde.

## <a name="next-steps"></a>Nächste Schritte

Navigieren zur Seite [Azure Active Directory (AD) – Preise](https://azure.microsoft.com/pricing/details/active-directory/)

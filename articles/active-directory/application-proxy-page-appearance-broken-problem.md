---
title: "Alkalmazás lapján nem jelennek meg helyesen az alkalmazásproxy-alkalmazás |} Microsoft Docs"
description: "Ha a lap nem megfelelően jelennek meg az alkalmazás Proxy alkalmazás útmutatást integrálva van az Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 017267620a1bcd6242bceb395966cc4190677c1a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Alkalmazás lapján nem jelennek meg helyesen az alkalmazásproxy-alkalmazáshoz

Ez a cikk segítséget az Azure Active Directory Alkalmazásproxyjával alkalmazásokkal kapcsolatos problémák elhárítása, az lapra lép, de az oldalon valami nem megfelelően jelenik meg helyes-e.

## <a name="overview"></a>Áttekintés
Alkalmazásproxy alkalmazások közzététele, amikor csak a legfelső szintű lapok érhetők el az alkalmazáshoz való hozzáféréskor. A lapon nem jelennek meg megfelelően, ha lehet, hogy a legfelső szintű belső használt URL-cím az alkalmazás hiányzik néhány lap erőforrást. Elhárítása érdekében ellenőrizze, hogy miután közzétette *összes* az erőforrásokat a lap az alkalmazás részeként.

Ellenőrizheti ezt a problémát a hálózati követő megnyitásával (például a Fiddler, vagy az F12 eszközök Internet Explorer vagy Edge), betölteni az oldalt, és a 404-es hibákat keres. Azt jelzi, hogy a lapok, amelyek jelenleg nem található, és előfordulhat, hogy továbbra is szeretné közzé kell tenni.

Ebben az esetben, például azt feltételezik, miután közzétette költségek kérelmet egy belső URL-címe használatával <http://myapps/expenses>, de az alkalmazás használ a stíluslap <http://myapps/style.css>. Ebben az esetben a stíluslap nincs közzétéve az alkalmazásban, a költségek alkalmazás betöltése kivételt a 404-es hibaüzenetet style.css betöltésére tett kísérlet közben. Ebben a példában a probléma szeretné feloldani egy belső URL-CÍMÉT az alkalmazás közzététele <http://myapp/> helyette.

## <a name="problems-with-publishing-as-one-application"></a>Egy alkalmazás közzétételi kapcsolatos problémák

Ha nincs lehetőség az ugyanazon alkalmazásban minden erőforrások közzétételét, módosítania több alkalmazások közzététele és a hivatkozások között.

Ha igen, azt javasoljuk, a [egyéni tartományok](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) megoldás. Azonban az ehhez a megoldáshoz szükségesek, hogy Ön a tulajdonosa a tanúsítványt a tartomány és az alkalmazások teljes tartományneveit (FQDN) használja. Egyéb lehetőségek, tekintse meg a [hivatkozások dokumentáció hibaelhárítása](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).

## <a name="next-steps"></a>Következő lépések
[Az Azure AD-alkalmazásproxy használó alkalmazások közzététele](application-proxy-publish-azure-portal.md)

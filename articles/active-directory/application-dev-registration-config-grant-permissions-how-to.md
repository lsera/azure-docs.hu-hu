---
title: "Hogyan kell megadni az engedélyeket az egyéni által fejlesztett alkalmazás |} Microsoft Docs"
description: "Hogyan engedélyezze, hogy az egyéni által fejlesztett alkalmazás használatával, az Azure AD portálon vagy az egy URL-paramétert"
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
ms.openlocfilehash: 0f13efd5e629bde592a3ab3a3e2db4a22180b5bb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a>Hogyan kell megadni az engedélyeket az egyéni által fejlesztett alkalmazás

Ha szeretné, hogy a hozzájárulás megelőző jelleggel meg az alkalmazást, vagy nem beleegyezett hiba történt az alkalmazások futnak, próbálkozzon az alábbi lépéseket.

## <a name="how-to-perform-admin-consent-for-your-application"></a>Az alkalmazás Admin hozzájárulási végrehajtása

Azt a hatását, hogy a szervezete összes felhasználója számára az alkalmazásba hozzájárulás megadása.

1. Keresse meg a **App regisztrációk** panelen, egy **globális rendszergazda**, majd válassza ki az alkalmazást.

2. Válassza ki **szükséges engedélyek**, majd végül nyomja le a **engedélyt adjon** gomb a panel tetején.

Alternatív megoldásként, hogyan hozhat létre a kérelmek *login.microsoftonline.com* a az alkalmazás configs és a hozzáfűzendő *& Rákérdezés = admin\_hozzájárulás*. Után rendszergazdai hitelesítő adataival bejelentkezni, az alkalmazás rendelkezik az összes felhasználó jóváhagyását.

## <a name="how-to-force-user-consent-for-your-application"></a>Az alkalmazás a felhasználói hozzájárulás kényszerítése

* Hitelesítési kérelmek alakzatot hozzáfűzése *& Rákérdezés hozzájárulási =* igénylő felhasználók számára, hogy hozzájárulás minden alkalommal, amikor a hitelesítéshez.

## <a name="next-steps"></a>Következő lépések

[Hozzájárulás és AzureAD alkalmazások integrálása](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

[Hozzájárulás és Permissioning AzureAD v2.0 az összevont alkalmazások](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)

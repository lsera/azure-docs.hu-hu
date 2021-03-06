---
title: "Több-bérlős háttérrendszerek áttekintése az Azure Application Gatewayjel | Microsoft Docs"
description: "Ez az oldal áttekintést nyújt az Application Gateway több-bérlős háttérrendszerekhez elérhető támogatásáról."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: 
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.openlocfilehash: d093af064bca46aa1f454b61b1099f47f61ccd33
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/03/2018
---
# <a name="application-gateway-support-for-multi-tenant-back-ends"></a>Az Application Gateway támogatása több-bérlős háttérrendszerekhez

Az Azure Application Gateway háttérkészleteiben virtuálisgép-méretezési csoportok, hálózati adapterek, nyilvános vagy magánhálózati IP-címek, illetve teljes tartománynevek (FQDN) szerepelhetnek. Alapértelmezés szerint az Application Gateway nem módosítja az ügyféltől származó bejövő HTTP-állomásfejlécet, és így küldi tovább a háttérrendszernek. Számos olyan szolgáltatás létezik, mint az [Azure Web Apps](../app-service/app-service-web-overview.md), amely eredendően több-bérlős szolgáltatás, és a megfelelő végpontra való feloldáshoz egy adott állomásfejlécet vagy SNI-bővítményt használ. Az Application Gateway mostantól támogatja a bejövő HTTP-állomásfejlécek felhasználói felülírásának lehetőségét a háttérrendszer HTTP-beállításai alapján. Ez a képesség teszi lehetővé a több-bérlős háttérrendszerek Azure Web Apps és API Management szolgáltatásának támogatását. Ez a képesség a standard és a WAF termékváltozat esetében is elérhető. A több-bérlős háttérrendszerek támogatása SSL-lezárások és teljes körű SSL-forgatókönyvek esetében is működik.

![webalkalmazás-forgatókönyv](./media/application-gateway-web-app-overview/scenario.png)

A gazdagép-felülbírálás meghatározása a HTTP-beállítások megadása során történik, és a szabálylétrehozás során bármely háttérkészletre alkalmazható. A több-bérlős háttérrendszerek az állomásfejlécek és az SNI-bővítmények alábbi két felülbírálási módját támogatják.

1. A gazdanév rögzített értékre történő beállítása a HTTP-beállítások megadása során. Ez a képesség biztosítja, hogy az állomásfejléc minden olyan háttérkészlet felé irányuló adatforgalom esetében erre az értékre legyen felülírva, amelyre a HTTP-beállítások vonatkoznak. Végpontok közötti SSL alkalmazása esetén az SNI-bővítményben a rendszer a felülírt gazdanevet használja. Ez a képesség lehetővé teszi olyan forgatókönyvek megvalósítását, ahol egy háttérkészletfarm a bejövő ügyfél állomásfejlécétől eltérő állomásfejlécet vár.

2. A gazdanév származtatása a háttérkészlettagok IP-címéből vagy teljes tartománynevéből. A HTTP-beállítások megadása során lehetőség van a gazdanév egy háttérkészlettag teljes tartománynevéből való kiválasztására, ha az úgy lett konfigurálva, hogy a gazdanév származtatása egy egyedi háttérkészlettagból történjen. Végpontok közötti SSL alkalmazása esetén az SNI-bővítményben a rendszer a teljes tartománynévből származtatott gazdanevet használja. Ez a képesség lehetővé teszi olyan forgatókönyvek megvalósítását, ahol a háttérkészlet kettő vagy több több-bérlős PaaS szolgáltatással is rendelkezhet. Ilyenek például az Azure Web Apps és a teljes tartománynévből származtatott gazdanevet tartalmazó tagokra irányuló kérések állomásfejlécei.

> [!NOTE]
> Mindkét előző esetben a beállítások csak az élő adatforgalomra vannak hatással, az állapotminta viselkedésére nem. Az egyéni minták már korábban is támogatták az állomásfejléc megadásának lehetőségét a mintavételi konfigurációban. Az egyéni minták mostantól támogatják az állomásfejléc viselkedésének az aktuálisan konfigurált HTTP-beállításokból történő származtatását is. Ezt a konfigurációt a mintavételi konfigurációban a `PickHostNameFromback endAddress` paraméterrel is megadhatjuk. A végpontok közötti funkciók működéséhez a mintavételi és a HTTP-beállításokat is úgy kell módosítani, hogy azok a megfelelő konfigurációt tükrözzék.

Ezzel a képességgel az ügyfelek megadhatják a HTTP-beállítások és az egyéni mintavételek beállítási lehetőségeit a megfelelő konfigurációhoz. Ezt a beállítást ezután a rendszer egy szabállyal egy adott figyelőhöz és háttérkészlethez köti.

## <a name="next-steps"></a>További lépések

Az Application Gateway háttérkészlet tagjaként funkcionáló webalkalmazással való beállításával kapcsolatban lásd [az App Service Web Apps Application Gatewayjel való beállításáról](application-gateway-web-app-powershell.md) szóló témakört.

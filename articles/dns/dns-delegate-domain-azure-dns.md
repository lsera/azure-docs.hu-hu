---
title: "Tartomány delegálása az Azure DNS-be | Microsoft Docs"
description: "Ismerje meg, hogyan módosíthatja a tartományok delegálását és használhatja tartományszolgáltatóként az Azure DNS-névkiszolgálóit."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: kumud
ms.openlocfilehash: a13d9b360e2c43aa2a3d4f62215e4570ce42cd5b
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/24/2018
---
# <a name="delegate-a-domain-to-azure-dns"></a>Tartomány delegálása az Azure DNS-be

Az Azure DNS használatával DNS-zónákat üzemeltethet, és kezelheti a tartomány DNS-rekordjait az Azure felületén. Egy tartomány DNS-lekérdezései csak akkor érik el az Azure DNS-t, ha a tartomány delegálva van az Azure DNS-be a szülőtartományból. Ne feledje: nem az Azure DNS a tartományregisztráló. Ez a cikk a tartomány delegálását ismerteti az Azure DNS-be.

A tartományregisztrálótól vásárolt tartományokhoz a regisztráló felajánlja, hogy beállítja a névkiszolgálói rekordokat. Nem kell ahhoz egy tartománnyal rendelkeznie, hogy az adott tartománynévvel létrehozzon egy DNS-zónát az Azure DNS-ben. Azonban a tartományregisztrálóval az Azure DNS-be való delegáláshoz már meg kell vásárolnia a tartományt.

Tegyük fel például, hogy megvette a „contoso.net” tartományt, és létrehozott egy „contoso.net” nevű zónát az Azure DNS-ben. Mivel Ön a tartomány tulajdonosa, a regisztráló felajánlja, hogy konfigurálja a tartomány névkiszolgálóinak címeit (azaz a névkiszolgálói rekordokat). A regisztráló ezeket a névkiszolgálói rekordokat a szülőtartományban, a „.net” tartományban tárolja. A világ különböző pontjain található ügyfelek ekkor az Azure DNS-zónabeli tartományához irányíthatók, amikor megpróbálják feloldani a „contoso.net” DNS-rekordjait.

## <a name="create-a-dns-zone"></a>DNS-zóna létrehozása

1. Jelentkezzen be az Azure portálra.
1. A **központi** menüben kattintson az **Új** > **Hálózatkezelés** > **DNS-zóna** elemre a **DNS-zóna létrehozása** lap megnyitásához.

   ![DNS-zóna](./media/dns-domain-delegation/dns.png)

1. A **DNS-zóna létrehozása** lapon adja meg a következő értékeket, majd kattintson a **Létrehozás** parancsra:

   | **Beállítás** | **Érték** | **Részletek** |
   |---|---|---|
   |**Name (Név)**|contoso.net|Adja meg a DNS-zóna nevét.|
   |**Előfizetés**|[Az Ön előfizetése]|Válasszon ki egy előfizetést, amelyben létrehozza az alkalmazásátjárót.|
   |**Erőforráscsoport**|**Új létrehozása:** contosoRG|Hozzon létre egy erőforráscsoportot. Az erőforráscsoport nevének egyedinek kell lennie a kiválasztott előfizetésen belül. Az erőforráscsoportokkal kapcsolatos további információért olvassa el az [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) áttekintését tartalmazó cikket.|
   |**Hely**|USA nyugati régiója||

> [!NOTE]
> Az erőforráscsoport helye nincs hatással a DNS-zónára. A DNS-zóna helye mindig „globális”, és nem jelenik meg.

## <a name="retrieve-name-servers"></a>Névkiszolgálók lekérdezése

Mielőtt DNS-zónáját az Azure DNS-be delegálhatná, meg kell tudnia a zóna névkiszolgálóinak nevét. Minden zóna létrehozásakor az Azure DNS egy névkiszolgálói készletből választ ki egyet.

1. Ha létrehozta a DNS-zónát, az Azure Portal **Kedvencek** ablaktábláján válassza az **Összes erőforrás** lehetőséget. Az **Összes erőforrás** lapon válassza a **contoso.net** DNS-zónát. Ha a kiválasztott előfizetésben már több erőforrás szerepel, az alkalmazásátjáró egyszerű eléréséhez beírhatja a **contoso.net** nevet a **Szűrés név alapján** mezőbe. 

1. Kérdezze le a névkiszolgálókat a DNS-zóna lapon. Ebben a példában a „contoso.net” zónához a következő névkiszolgálók tartoznak: „ns1-01.azure-dns.com”, „ns2-01.azure-dns.net”, „ns3-01.azure-dns.org” és „ns4-01.azure-dns.info”:

   ![Névkiszolgálók listája](./media/dns-domain-delegation/viewzonens500.png)

Az Azure DNS automatikusan létrehozza a zóna mérvadó névkiszolgálói rekordjait, amelyek a zónához rendelt névkiszolgálókat tartalmazzák. A névkiszolgálókat az Azure PowerShellen vagy az Azure CLI-n keresztül is megtekintheti ezeknek a rekordoknak a lekérésével.

Az alábbi példák bemutatják az egyes zónák névkiszolgálói lekérdezésének lépéseit az Azure DNS-ben a PowerShell és az Azure CLI használatával.

### <a name="powershell"></a>PowerShell

```powershell
# The record name "@" is used to refer to records at the top of the zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

A következő példa a válasz:

```
Name              : @
ZoneName          : contoso.net
ResourceGroupName : contosorg
Ttl               : 172800
Etag              : 03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5
RecordType        : NS
Records           : {ns1-07.azure-dns.com., ns2-07.azure-dns.net., ns3-07.azure-dns.org.,
                    ns4-07.azure-dns.info.}
Metadata          :
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az network dns record-set list --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

A következő példa a válasz:

```json
{
  "etag": "03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosoRG/providers/Microsoft.Network/dnszones/contoso.net/NS/@",
  "metadata": null,
  "name": "@",
  "nsRecords": [
    {
      "nsdname": "ns1-07.azure-dns.com."
    },
    {
      "nsdname": "ns2-07.azure-dns.net."
    },
    {
      "nsdname": "ns3-07.azure-dns.org."
    },
    {
      "nsdname": "ns4-07.azure-dns.info."
    }
  ],
  "resourceGroup": "contosoRG",
  "ttl": 172800,
  "type": "Microsoft.Network/dnszones/NS"
}
```

## <a name="delegate-the-domain"></a>A tartomány delegálása

Most, hogy létrehozta a DNS-zónát, és megvannak a névkiszolgálók is, frissítenie kell a szülőtartományt az Azure DNS-névkiszolgálókkal. Minden tartományregisztráló a saját DNS-kezelési eszközeit használja a tartományok névkiszolgálói rekordjainak módosítására. A regisztráló DNS-kezelési oldalán szerkessze a névkiszolgálói rekordokat, és cserélje le őket az Azure DNS által létrehozottakra.

Amikor egy tartományt az Azure DNS-be delegál, az Azure DNS által nyújtott névkiszolgálókat kell használnia. Javasoljuk, hogy használja mind a négy névkiszolgálói nevet, a tartomány nevétől függetlenül. A tartománydelegáláshoz nem szükséges, hogy a névkiszolgáló ugyanazt a legfelső szintű tartományt használja, mint az Ön tartománya.

Ne használjon *összekapcsoló rekordokat* az Azure DNS névkiszolgálói IP-címeire való rámutatáshoz, mert ezek az IP-címek megváltozhatnak a jövőben. A saját zónájában történő, névkiszolgálókat használó delegálásokat – más néven *személyes névkiszolgálókat* – az Azure DNS jelenleg nem támogatja.

## <a name="verify-that-name-resolution-is-working"></a>A névfeloldás működésének ellenőrzése

A delegálás befejezése után ellenőrizheti, hogy a névfeloldás működik-e. Ezt például az nslookup vagy egy hasonló eszköz segítségével teheti meg, amely lekérdezi a zónája SOA típusú rekordját. (A SOA típusú rekord automatikusan létrejön a zóna létrehozásakor.)

Az Azure DNS névkiszolgálóit nem kell megadnia. Ha a delegálást helyesen végezte el, a hagyományos DNS-feloldási folyamat automatikusan megtalálja a névkiszolgálókat.

```
nslookup -type=SOA contoso.com
```

Az alábbiakban egy példát láthat az előző parancsra adott válaszra:

```
Server: ns1-04.azure-dns.com
Address: 208.76.47.4

contoso.com
primary name server = ns1-04.azure-dns.com
responsible mail addr = msnhst.microsoft.com
serial = 1
refresh = 900 (15 mins)
retry = 300 (5 mins)
expire = 604800 (7 days)
default TTL = 300 (5 mins)
```

## <a name="delegate-subdomains-in-azure-dns"></a>Delegate subdomains in Azure DNS

Ha különálló gyermekzónát szeretne létrehozni, azt megteheti egy altartomány Azure DNS-beli delegálásával. Tegyük fel például, hogy a beállította és delegálta az Azure DNS-ben a „contoso.net” tartományt. Ezután most egy különálló gyermekzónát szeretne létrehozni, „partners.contoso.net” néven.

1. Hozza létre a „partners.contoso.net” gyermekzónát az Azure DNS-ben.
2. Keresse meg a mérvadó névkiszolgálói rekordokat a gyermekzónában, így megtalálja a gyermekzónát az Azure DNS-ben üzemeltető névkiszolgálókat.
3. A gyermekzónára mutató szülőzónában delegálja a gyermekzónát a névkiszolgálói rekordok konfigurálásával.

### <a name="create-a-dns-zone"></a>DNS-zóna létrehozása

1. Jelentkezzen be az Azure portálra.
1. A **központi** menüben kattintson az **Új** > **Hálózatkezelés** > **DNS-zóna** elemre a **DNS-zóna létrehozása** lap megnyitásához.

   ![DNS-zóna](./media/dns-domain-delegation/dns.png)

1. A **DNS-zóna létrehozása** lapon adja meg a következő értékeket, majd kattintson a **Létrehozás** parancsra:

   | **Beállítás** | **Érték** | **Részletek** |
   |---|---|---|
   |**Name (Név)**|partners.contoso.net|Adja meg a DNS-zóna nevét.|
   |**Előfizetés**|[Az Ön előfizetése]|Válasszon ki egy előfizetést, amelyben létrehozza az alkalmazásátjárót.|
   |**Erőforráscsoport**|**Meglévő használata:** contosoRG|Hozzon létre egy erőforráscsoportot. Az erőforráscsoport nevének egyedinek kell lennie a kiválasztott előfizetésen belül. Az erőforráscsoportokkal kapcsolatos további információkért olvassa el [A Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) áttekintése című cikket.|
   |**Hely**|USA nyugati régiója||

> [!NOTE]
> Az erőforráscsoport helye nincs hatással a DNS-zónára. A DNS-zóna helye mindig „globális”, és nem jelenik meg.

### <a name="retrieve-name-servers"></a>Névkiszolgálók lekérdezése

1. Ha létrehozta a DNS-zónát, az Azure Portal **Kedvencek** ablaktábláján válassza az **Összes erőforrás** lehetőséget. Válassza ki a **partners.contoso.net** DNS-zónát az **Összes erőforrás** lapon. Ha a kiválasztott előfizetésben már több erőforrás szerepel, a DNS-zóna egyszerű eléréséhez beírhatja a **partners.contoso.net** nevet a **Szűrés név alapján** mezőbe.

1. Kérdezze le a névkiszolgálókat a DNS-zóna lapon. Ebben a példában a „contoso.net” zónához a következő névkiszolgálók tartoznak: „ns1-01.azure-dns.com”, „ns2-01.azure-dns.net”, „ns3-01.azure-dns.org” és „ns4-01.azure-dns.info”:

   ![Névkiszolgálók listája](./media/dns-domain-delegation/viewzonens500.png)

Az Azure DNS automatikusan létrehozza a zóna mérvadó névkiszolgálói rekordjait, amelyek a zónához rendelt névkiszolgálókat tartalmazzák.  A névkiszolgálókat az Azure PowerShellen vagy az Azure CLI-n keresztül is megtekintheti ezeknek a rekordoknak a lekérésével.

### <a name="create-a-name-server-record-in-the-parent-zone"></a>Névkiszolgáló-rekord létrehozása a szülőzónában

1. Az Azure Portalon lépjen a **contoso.net** DNS-zónára.
1. Kattintson a **+ Rekordhalmaz** gombra.
1. A **Rekordhalmaz hozzáadása** lapon adja meg az alábbi értékeket, majd kattintson az **OK** gombra.

   | **Beállítás** | **Érték** | **Részletek** |
   |---|---|---|
   |**Name (Név)**|partners|Adja meg a gyermek DNS-zóna nevét.|
   |**Típus**|NS|A névkiszolgálók esetében használja az NS típust.|
   |**TTL**|1|Adja meg az élettartamot.|
   |**TTL mértékegysége**|Óra|Állítsa be, hogy az élettartam mértékegysége az óra legyen.|
   |**NÉVKISZOLGÁLÓ**|{névkiszolgálók a partners.contoso.net zónából}|Adja meg mind a négy névkiszolgálót a partners.contoso.net zónából. |

   ![A névkiszolgálói rekord értékei](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegate-subdomains-in-azure-dns-by-using-other-tools"></a>Altartományok delegálása az Azure DNS-ben más eszközökkel

Az alábbi példák az altartományok delegálásának lépéseit mutatják be az Azure DNS-ben a PowerShell és az Azure CLI használatával:

#### <a name="powershell"></a>PowerShell

Az alábbi PowerShell-példa bemutatja ennek működését. Ugyanezek a lépések az Azure Portalon vagy a platformfüggetlen Azure CLI eszközön keresztül is végrehajthatók.

```powershell
# Create the parent and child zones. These can be in the same resource group or different resource groups, because Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve the authoritative NS records from the child zone as shown in the next example. This information contains the name servers assigned to the child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create the corresponding NS record set in the parent zone to complete the delegation. The record set name in the parent zone matches the child zone name (in this case, "partners").
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

Az `nslookup` használatával a gyermekzóna SOA típusú rekordjának megkeresésével ellenőrizze, hogy minden helyesen van-e beállítva.

```
nslookup -type=SOA partners.contoso.com
```

```
Server: ns1-08.azure-dns.com
Address: 208.76.47.8

partners.contoso.com
    primary name server = ns1-08.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
#!/bin/bash

# Create the parent and child zones. These can be in the same resource group or different resource groups, because Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

Kérdezze le a `partners.contoso.net` zóna névkiszolgálóit a kimenetből.

```
{
  "etag": "00000003-0000-0000-418f-250de2b2d201",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosorg/providers/Microsoft.Network/dnszones/partners.contoso.net",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "partners.contoso.net",
  "nameServers": [
    "ns1-09.azure-dns.com.",
    "ns2-09.azure-dns.net.",
    "ns3-09.azure-dns.org.",
    "ns4-09.azure-dns.info."
  ],
  "numberOfRecordSets": 2,
  "resourceGroup": "contosorg",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Hozza létre a rekordkészletet és a névkiszolgálói rekordokat mindegyik névkiszolgálóhoz.

```azurecli
#!/bin/bash

# Create the record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create an NS record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a>Az összes erőforrás törlése

A jelen cikkben létrehozott összes erőforrás törléséhez hajtsa végre az alábbi lépéseket:

1. Az Azure Portal **Kedvencek** ablaktábláján válassza az **Összes erőforrás** lehetőséget. Az **Összes erőforrás** lapon válassza a **contosorg** erőforráscsoportot. Ha a kiválasztott előfizetésben már több erőforrás szerepel, az erőforráscsoport egyszerű eléréséhez beírhatja a **contosorg** nevet a **Szűrés név alapján** mezőbe.
1. A **contosorg** lapon kattintson a **Törlés** gombra.
1. A portál megköveteli, hogy az erőforráscsoport törlésének megerősítéséhez beírja annak nevét. Írja be a **contosorg** nevet az erőforráscsoport nevéhez, majd kattintson a **Törlés** gombra. 

Egy erőforráscsoport törlése a csoportban található összes erőforrást törli. Mindig ellenőrizze az erőforráscsoportok tartalmát azok törlése előtt. A portál törli az erőforráscsoportban található összes erőforrást, majd magát az erőforráscsoportot is. Ez a folyamat több percig is eltarthat.

## <a name="next-steps"></a>További lépések

[DNS-zónák kezelése](dns-operations-dnszones.md)

[DNS-rekordok kezelése](dns-operations-recordsets.md)

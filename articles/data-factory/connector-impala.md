---
title: "Adatok másolása az Impala Azure Data Factory (béta) használatával |} Microsoft Docs"
description: "Útmutató: adatok másolása Impala támogatott fogadó adattárolókhoz a másolási tevékenység használatával a data factory-folyamathoz."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: e1f745fc70395f06d2eb3d98644d54c314a0ef26
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/09/2018
---
# <a name="copy-data-from-impala-by-using-azure-data-factory-beta"></a>Adatok másolása az Impala Azure Data Factory (béta) használatával

Ez a cikk a másolási tevékenység használható az Azure Data Factory adatokat másolni Impala módját ismerteti. Buildekről nyújtanak a [másolási tevékenység áttekintése](copy-activity-overview.md) cikket, amely megadja a másolási tevékenység általános áttekintést.

> [!NOTE]
> Ez a cikk a Data Factory 2. verziójára vonatkozik, amely jelenleg előzetes verzióban érhető el. Ha a Data Factory, amely általában a rendelkezésre álló, 1 verzióját használja [másolási tevékenység az 1-es verziójú](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Ez az összekötő jelenleg bétaverziójú. Próbálja ki, és visszajelzést. Ne használja éles környezetben.

## <a name="supported-capabilities"></a>Támogatott képességei

Adatok bármely támogatott fogadó adattárolóhoz Impala másolhatja. A másolási tevékenység által támogatott adatforrások vagy mosdók adattárolókhoz listájáért lásd: a [adattárolókhoz támogatott](copy-activity-overview.md#supported-data-stores-and-formats) tábla.

 Adat-előállító kapcsolódásának engedélyezése beépített illesztőprogramot tartalmaz. Emiatt nem kell manuálisan a ezzel az összekötővel az illesztőprogram telepítéséhez.

## <a name="get-started"></a>Bevezetés

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

A következő szakaszok részletesen bemutatják adat-előállító tartozó entitások megadhatók a Impala összekötőhöz használt tulajdonságokat.

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok

A következő tulajdonságok Impala társított szolgáltatás esetén támogatottak.

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type | A type tulajdonságot meg kell **Impala**. | Igen |
| gazdagép | A kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét a Impala (Ez azt jelenti, hogy 192.168.222.160).  | Igen |
| port | A TCP-portot, amelyen a Impala kiszolgáló ügyfélkapcsolatokat. Az alapértelmezett érték: 21050.  | Nem |
| authenticationType | A használandó hitelesítési típus. <br/>Két érték engedélyezett **névtelen**, **SASLUsername**, és **UsernameAndPassword**. | Igen |
| felhasználónév | A felhasználónév, a Impala kiszolgálóhoz való hozzáféréshez. Az alapértelmezett értéke névtelen SASLUsername használatakor.  | Nem |
| jelszó | A jelszót, amely megfelel a felhasználó nevét UsernameAndPassword használatakor. Ez a mező megjelölése a SecureString tárolja biztonságos helyen, a Data factoryban vagy [hivatkozik az Azure Key Vault tárolt titkos kulcs](store-credentials-in-key-vault.md). | Nem |
| enableSsl | Meghatározza, hogy a kapcsolat SSL használatával titkosított. Az alapértelmezett érték **hamis**.  | Nem |
| trustedCertPath | A teljes elérési útja a .pem fájl, amely tartalmazza a megbízható Hitelesítésszolgáltatói tanúsítvány segítségével ellenőrizze a kiszolgáló SSL-en keresztül csatlakoztatásakor. Ez a tulajdonság csak akkor, ha az SSL használatát Self-hosted integrációs futásidejű állítható be. Az alapértelmezett érték: a telepített integrációs futásidejű cacerts.pem fájlt.  | Nem |
| useSystemTrustStore | Megadja, hogy a rendszer megbízható áruházból vagy a megadott PEM-fájl egy Hitelesítésszolgáltatói tanúsítványt használjon-e. Az alapértelmezett érték **hamis**.  | Nem |
| allowHostNameCNMismatch | Megadja, hogy egy hitelesítésszolgáltató által kiállított SSL tanúsítvány nevének egyeznie kell a gazdagép nevével a kiszolgáló SSL-en keresztül csatlakoztatásakor-e. Az alapértelmezett érték **hamis**.  | Nem |
| allowSelfSignedServerCert | Megadja, hogy engedélyezi a kiszolgáló önaláírt tanúsítványokat. Az alapértelmezett érték **hamis**.  | Nem |
| connectVia | A [integrációs futásidejű](concepts-integration-runtime.md) csatlakozni az adattárolóhoz használandó. Használhatja Self-hosted integrációs futásidejű vagy Azure integrációs futásidejű (ha az adattároló nyilvánosan elérhető). Ha nincs megadva, akkor használja az alapértelmezett Azure integrációs futásidejű. |Nem |

**Példa**

```json
{
    "name": "ImpalaLinkedService",
    "properties": {
        "type": "Impala",
        "typeProperties": {
            "host" : "<host>",
            "port" : "<port>",
            "authenticationType" : "UsernameAndPassword",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai

Szakaszok és meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek](concepts-datasets-linked-services.md) cikk. Ez a témakör a Impala dataset által támogatott tulajdonságokról.

Adatok másolása Impala, állítsa be a type tulajdonságot az adathalmaz **ImpalaObject**. Nincs ilyen típusú dataset további típusra vonatkozó tulajdonság.

**Példa**

```json
{
    "name": "ImpalaDataset",
    "properties": {
        "type": "ImpalaObject",
        "linkedServiceName": {
            "referenceName": "<Impala linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai

Szakaszok és a rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [folyamatok](concepts-pipelines-activities.md) cikk. Ez a témakör a Impala forrástípus által támogatott tulajdonságokról.

### <a name="impala-as-a-source-type"></a>A forrás típusaként impala

Adatok másolása Impala, állítsa be a forrás típusa a másolási tevékenység **ImpalaSource**. A következő tulajdonságok támogatottak a másolási tevékenység **forrás** szakasz.

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type | A type tulajdonságot a másolási tevékenység forrás értékre kell állítani **ImpalaSource**. | Igen |
| lekérdezés | Az egyéni SQL-lekérdezés segítségével adatokat olvasni. Például: `"SELECT * FROM MyTable"`. | Igen |

**Példa**

```json
"activities":[
    {
        "name": "CopyFromImpala",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Impala input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "ImpalaSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>További lépések
Források és mosdók adat-előállítóban másolási tevékenység által támogatott adattárolókhoz listájáért lásd: [adattárolókhoz támogatott](copy-activity-overview.md#supported-data-stores-and-formats).

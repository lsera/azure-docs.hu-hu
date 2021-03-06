---
title: "Azure IoT Hub üzenetformátum megértése |} Microsoft Docs"
description: "Fejlesztői útmutató - descibes a formátum és az IoT-központ üzenetek várt tartalom."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 3d5b500964ee37dbd347858edd35812e1d217499
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/01/2018
---
# <a name="create-and-read-iot-hub-messages"></a>Hozzon létre, és az IoT-központ üzenet olvasása

Zökkenőmentes együttműködés támogatására biztosíthat a protokollokon, IoT-központ az összes eszköz számára is elérhető protokollhoz közös üzenetformátum határozza meg. Mindkét használt üzenetformátuma [eszközről a felhőbe] [ lnk-d2c] és [felhő eszközre] [ lnk-c2d] üzeneteket. Egy [IoT-központ üzenet] [ lnk-messaging] áll:

* Egy *Rendszertulajdonságok*. Az IoT-központ értelmezi, vagy beállítja a tulajdonságokat. A csoportok pedig előre meghatározott.
* Egy *alkalmazástulajdonságok*. Az alkalmazás meghatározó karakterlánc-tulajdonságok és a hozzáférés, anélkül, hogy az üzenet törzsének deszerializálása dictionary. Az IoT-központ soha nem módosítja ezeket a tulajdonságokat.
* Nem átlátszó bináris törzsében.

Nevét és értékeit csak ASCII alfanumerikus karaktereket tartalmazhat, valamint ```{'!', '#', '$', '%, '&', "'", '*', '+', '-', '.', '^', '_', '`', '|', '~'}``` amikor Ön:  

* A HTTPS protokoll használatával eszközről a felhőbe üzeneteket küldeni.
* Felhő-eszközre küldött üzenetek küldése.

Kódolására, és különböző protokollok használatával küldött üzenetek dekódolási kapcsolatos további információkért lásd: [Azure IoT SDK-k][lnk-sdks].

A következő táblázat az IoT Hub-kezelő üzeneteinek tulajdonságainak listája.

| Tulajdonság | Leírás |
| --- | --- |
| MessageId |Az üzenet kérelem-válasz minták használt felhasználói állítható be azonosítója. Formátum: A kis-és nagybetűket (legfeljebb 128 karakter hosszú) ASCII 7 bites alfanumerikus karakterekből álló karakterlánc + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Sorozat száma |Minden felhő eszközre üzenetet az IoT-központ által hozzárendelt szám (eszköz-várólista minden egyedi). |
| Művelet |A megadott célhelyre [felhő eszközre] [ lnk-c2d] üzeneteket. |
| ExpiryTimeUtc |Dátum és az üzenet lejárati idejét. |
| EnqueuedTime |Dátum és idő a [felhő eszközre] [ lnk-c2d] üzenet érkezett az IoT-központot. |
| CorrelationId |A kérelem a kérelem-válasz minták MessageId általában tartalmazó válaszüzenetet a karakterlánc típusú tulajdonság. |
| UserId |Adja meg az üzenetek eredeti használt azonosító. Az IoT-központ által előállított üzeneteket, ha van-e beállítva `{iot hub name}`. |
| Nyugtázási |A visszajelzési üzenet generátor. Ezt a tulajdonságot használják a felhő-eszközre küldött üzenetek igényelni az IoT-központ létrehozhat visszajelzés üzeneteket a felhasználás az üzenet miatt az eszköz. A lehetséges értékek: **nincs** (alapértelmezett): Nincs visszajelzés üzenet jön létre, **pozitív**: visszajelzés üzenetet kap, ha az üzenet befejeződött, **negatív**: visszajelzés üzenetet kap, ha nélkül végzi az eszközt, az üzenet lejárt (vagy elérte a maximális száma) vagy **teljes**: pozitív és negatív. További információkért lásd: [visszajelzés üzenet][lnk-feedback]. |
| ConnectionDeviceId |Az eszköz a felhőbe küldött üzeneteket az IoT-központ által beállított azonosító. Tartalmazza a **deviceId** az eszközt, az üzenetet küldő. |
| ConnectionDeviceGenerationId |Az eszköz a felhőbe küldött üzeneteket az IoT-központ által beállított azonosító. Tartalmazza a **generationId** (megfelelően [identitás eszköztulajdonságok][lnk-device-properties]) az eszköz az üzenetet küldő. |
| ConnectionAuthMethod |Az eszköz a felhőbe küldött üzeneteket az IoT-központ által beállított hitelesítési módszert. Ez a tulajdonság a üzenetet küld az eszköz hitelesítésére használt hitelesítési módszert információkat tartalmaz. További információkért lásd: [hamisításszűrés felhőbe eszköz][lnk-antispoofing]. |

## <a name="message-size"></a>Üzenet mérete

Az IoT-központ méri üzenet mérete protokoll-független módon annak eldöntéséhez, hogy csak a tényleges tartalmat. A mérete bájtban a következő összegeként számítható ki:

* A törzs mérete bájtban.
* A mérete bájtban megadva az üzenet tulajdonságainak értékek.
* A mérete bájtban minden felhasználó nevét és értékeit.

Nevét és értékeit korlátozódnak ASCII-karaktereket, így a karakterlánc hosszát eredménye a mérete bájtban.

## <a name="next-steps"></a>További lépések

További információ a méretkorlátozásokról üzenetet az IoT hubon: [IoT-központ kvóták és sávszélesség-szabályozási][lnk-quotas].

Megtudhatja, hogyan hozhat létre, és az IoT-központ különböző programnyelveken üzenet olvasása, tekintse meg a [Ismerkedés] [ lnk-get-started] oktatóanyagok.

[lnk-messaging]: iot-hub-devguide-messaging.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-feedback]: iot-hub-devguide-messages-c2d.md#message-feedback
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-antispoofing]: iot-hub-devguide-messages-d2c.md#anti-spoofing-properties

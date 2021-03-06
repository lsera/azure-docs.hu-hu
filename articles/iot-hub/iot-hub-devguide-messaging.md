---
title: "Azure IoT Hub üzenetküldési megértése |} Microsoft Docs"
description: "Fejlesztői útmutató - eszközről a felhőbe és a felhő-eszközt az IoT-központ az üzenetküldési. Üzenetformátumok és a támogatott kommunikációs protokollok kapcsolatos adatokat tartalmaz."
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
ms.openlocfilehash: 7fd89bebf9d7497ad5b13c438b362256d3408219
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/01/2018
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a>Eszköz-felhő és a felhő-eszközt az IoT-központ az üzenetküldési

Az IoT-központ az eszközök által kommunikálni üzenetküldési használja:

* Küldés [eszközről a felhőbe] [ lnk-d2c] háttér üzeneteket az eszközökről a megoldáshoz.
* Küldés [felhő eszközre] [ lnk-c2d] a megoldás üzeneteit háttér az eszközökön.

Az IoT-központ üzenetküldési funkció Core tulajdonságai, a megbízhatóság és a tartós üzenetek. Ezek a Tulajdonságok időszakos csatlakozási a eszköz oldalon, és a felhő oldalán Eseményfeldolgozási igényeiben jelentkező betöltése a rugalmasság engedélyezése. Az IoT-központ megvalósítja *legalább egyszer* kézbesítési biztosítja, hogy az eszközről a felhőbe és a felhő eszközre üzenetek.

A funkciókat az IoT-központot, témakörben megismerkedhet a cikkek [Azure és az eszközök internetes hálózatát] [ lnk-azure-iot] és [az Azure IoT-központ szolgáltatás áttekintése][lnk-iot-hub-overview].

## <a name="when-to-use-iot-hub-messaging"></a>Üzenetküldés az IoT-központ használatával

Használja az eszközről a felhőbe üzenetek küldésének ideje adatsorozat telemetriai adatok és riasztások az eszköz alkalmazásból, és a felhő-eszközre küldött üzenetek egyirányú értesítéseket csak az eszköz alkalmazás.

* Tekintse meg [eszközről a felhőbe kommunikációs útmutatást] [ lnk-d2c-guidance] Ha bizonytalan az eszköz a felhőbe küldött üzeneteket, a jelentett tulajdonságok vagy a fájl feltöltése között.
* Tekintse meg [felhő eszközre kommunikációs útmutatást] [ lnk-c2d-guidance] Ha kétségei vannak, a felhő-eszközre küldött üzenetek, kívánt tulajdonságokkal vagy közvetlen módszerek között.

## <a name="next-steps"></a>További lépések

* További tudnivalók az IoT-központ [eszközről a felhőbe üzenetküldési][lnk-d2c].
* További tudnivalók az IoT-központ [eszközre cloud messaging][lnk-c2d].

[lnk-azure-iot]: iot-hub-what-is-azure-iot.md
[lnk-iot-hub-overview]: iot-hub-what-is-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
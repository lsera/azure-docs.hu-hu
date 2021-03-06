---
title: "Az Azure Service Fabric CLI parancsfájl eltávolítása minta"
description: "Alkalmazások eltávolítása egy Azure Service Fabric-fürt, az Azure Service Fabric parancssori felület használatával"
services: service-fabric
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 95a6bac7ee79e3490584305e78d410104363b780
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/08/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Alkalmazások eltávolítása a Service Fabric-fürt

Ez a parancsfájlpélda futó Service Fabric-alkalmazás példány törlése, regisztrációjának törlése az alkalmazástípus és -verzió a fürtből.  Az alkalmazáspéldány törlésekor is törlődnek a futó szolgáltatás az adott alkalmazáshoz tartozó példányok. Ezt követően az alkalmazásfájlokat az image store törlődnek. 

Szükség esetén telepítse a [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Mintaparancsfájl

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Következő lépések

További információkért lásd: a [Service Fabric CLI dokumentáció](../service-fabric-cli.md).

Azure Service Fabric további Service Fabric CLI-példák találhatók a [Service Fabric CLI minták](../samples-cli.md).

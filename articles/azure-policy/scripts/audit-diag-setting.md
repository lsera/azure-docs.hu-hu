---
title: "Az Azure házirend json-mintát - diagnosztikai naplózása |} Microsoft Docs"
description: "A json-mintát házirend eseményeket, ha a diagnosztikai beállítások nincs engedélyezve a megadott erőforrástípus."
services: azure-policy
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 
ms.service: azure-policy
ms.devlang: 
ms.topic: sample
ms.tgt_pltfrm: 
ms.workload: 
ms.date: 10/30/2017
ms.author: banders
ms.custom: mvc
ms.openlocfilehash: 0e97470491f548cafd5023851e2c2f6bd76cbe9f
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/14/2017
---
# <a name="audit-diagnostic-setting"></a>Diagnosztikai naplózás beállítása

Naplózás, ha a diagnosztikai beállítások nincs engedélyezve a megadott típusú erőforrások. Megadja annak ellenőrzése, hogy engedélyezve vannak-e a diagnosztikai beállítások tömbjét.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Minta sablon

[!code-json[main](../../../policy-templates/samples/Monitoring/audit-diagnostic-setting/azurepolicy.json "Audit diagnostic setting")]

A sablon használatával telepíthető a [Azure-portálon](#deploy-with-the-portal), a [PowerShell](#deploy-with-powershell) vagy a [Azure CLI](#deploy-with-azure-cli).

## <a name="deploy-with-the-portal"></a>Üzembe helyezés a portállal

[![Üzembe helyezés az Azure-ban](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FMonitoring%2Faudit-diagnostic-setting%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>Üzembe helyezés a PowerShell-lel

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "audit-diagnostic-setting" -DisplayName "Audit diagnostic setting" -description "Audit diagnostic setting for selected resource types" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Monitoring/audit-diagnostic-setting/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Monitoring/audit-diagnostic-setting/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -listOfResourceTypes <Resource Types> -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>PowerShell központi telepítés tisztítása

A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Üzembe helyezés az Azure parancssori felületén keresztül

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy definition create --name 'audit-diagnostic-setting' --display-name 'Audit diagnostic setting' --description 'Audit diagnostic setting for selected resource types' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Monitoring/audit-diagnostic-setting/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Monitoring/audit-diagnostic-setting/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "audit-diagnostic-setting"
```

### <a name="clean-up-azure-cli-deployment"></a>Az Azure parancssori felület központi telepítés tisztítása

A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Következő lépések

- További Azure csoportházirend-sablon minták erővel [sablonok Azure házirend](../json-samples.md).

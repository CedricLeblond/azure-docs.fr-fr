---
title: "Ajouter ou supprimer des cartes réseau de machines virtuelles Azure | Documents Microsoft"
description: "Découvrez comment ajouter ou supprimer des interfaces réseau de machines virtuelles."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: jdial
ms.openlocfilehash: 7df1dfbea8c985907d5330819dc1e7bf1578aafa
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2017
---
# <a name="add-network-interfaces-to-or-remove-from-virtual-machines"></a>Ajouter ou supprimer des cartes réseau de machines virtuelles

Découvrez comment ajouter une interface réseau existante lors de la création d’une machine virtuelle, ou ajouter ou supprimer des interfaces réseau d’une machine virtuelle existante à l’état arrêté (désalloué). Une interface réseau permet à une machine virtuelle Azure de communiquer avec des ressources sur Internet, sur Azure et locales. Une machine virtuelle peut avoir une ou plusieurs interfaces réseau. 

Si vous avez besoin d’ajouter, de modifier ou de supprimer des adresses IP pour une interface réseau, consultez l’article [Ajouter, modifier ou supprimer des adresses IP pour des cartes réseau Azure](virtual-network-network-interface-addresses.md). Si vous avez besoin de créer, modifier ou supprimer des interfaces réseau, consultez l’article [Gérer les interfaces réseau](virtual-network-network-interface.md).

## <a name="before"></a>Avant de commencer

Avant d’effectuer une étape de cet article, quelle que soit sa section, effectuez les tâches suivantes :

- Pour en savoir plus sur le nombre d’interfaces réseau que chaque taille de machine virtuelle Linux et Windows prend en charge, voir les articles sur les tailles de machine virtuelle [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Connectez-vous au [portail](https://portal.azure.com) Azure, à l’interface de ligne de commande (CLI) Azure ou à Azure PowerShell avec un compte Azure. Si vous n’avez pas encore de compte, inscrivez-vous pour bénéficier d’un [essai gratuit](https://azure.microsoft.com/free).
- Si vous utilisez des commandes PowerShell pour accomplir les tâches décrites dans cet article, vous devez [Installer et configurer Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Assurez-vous que les applets de commande Azure PowerShell installées sont celles de la version la plus récente. Pour obtenir de l’aide sur les commandes PowerShell ainsi que des exemples, entrez `get-help <command> -full`.
- Si vous utilisez des commandes de l’interface de ligne de commande (CLI) Azure pour accomplir les tâches décrites dans cet article, [installez et configurez Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Assurez-vous que la version la plus récente d’Azure CLI est installée. Pour obtenir de l’aide sur les commandes CLI, entrez `az <command> --help`. Au lieu d’installer CLI et ses prérequis, vous pouvez utiliser Azure Cloud Shell. Azure Cloud Shell est un interpréteur de commandes Bash gratuit, que vous pouvez exécuter directement dans le portail Azure. L’interface Azure CLI est préinstallée et configurée pour être utilisée avec votre compte. Pour utiliser Cloud Shell, cliquez sur le bouton Cloud Shell **>_** en haut du [portail](https://portal.azure.com).

## <a name="about"></a>À propos des interfaces réseau et des machines virtuelles

Vous pouvez ajouter (attacher) une interface réseau existante à une machine virtuelle lorsque vous créez celle-ci, pour autant que l’interface réseau ne soit pas actuellement attachée à une autre machine virtuelle. Vous pouvez ajouter ou supprimer (détacher) une interface réseau de machine virtuelle existante, pour autant que celle-ci soit à l’état arrêté (désalloué). Si vous créez une machine virtuelle via le portail Azure, celui-ci crée pour vous une interface réseau avec des paramètres par défaut. Le portail ne vous autorise pas à :

- Spécifier une interface réseau existante à ajouter lors de la création de la machine virtuelle
- Créer une machine virtuelle avec plusieurs interfaces réseau
- Spécifiez le nom de l’interface réseau (le portail crée l’interface réseau avec un nom par défaut)

Vous pouvez utiliser Azure PowerShell ou Azure CLI pour créer une machine virtuelle ou une interface réseau avec tous les attributs précédents pour lesquels vous ne pouvez pas utiliser le portail. Avant d’effectuer les tâches décrites dans les sections ci-après, tenez compte des contraintes et des comportements suivants :

- Toutes les tailles de machine virtuelle prennent en charge au moins deux interfaces réseau, mais certaines en prennent en charge plus de deux. Dans le passé, certaines tailles de machine virtuelle ne prenaient en charge qu’une seule interface réseau. Pour découvrir le nombre d’interfaces réseau prises en charge par chaque taille de machine virtuelle, lisez les articles sur les tailles des machines virtuelles [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 
- Auparavant, les interfaces réseau ne pouvaient être ajoutées qu’aux machines virtuelles prenant en charge plusieurs interfaces réseau et créées avec au moins deux interfaces réseau. Il n’était pas possible d’ajouter une interface réseau à une machine virtuelle créée avec une seule interface réseau, même si la taille de machine virtuelle prenait en charge plusieurs interfaces réseau. À l’inverse, vous pouviez uniquement supprimer des interfaces réseau à partir d’une machine virtuelle comportant au moins trois interfaces réseau, car les machines virtuelles créées avec au moins deux interfaces réseau devaient toujours avoir au moins deux interfaces réseau. Ces contraintes sont aujourd’hui obsolètes. Vous pouvez maintenant créer une machine virtuelle avec un nombre quelconque d’interfaces réseau (pour autant que ce nombre soit pris en charge par la taille de machine virtuelle) et ajouter ou supprimer un nombre quelconque d’interfaces réseau (de machines virtuelles en état arrêté (désalloué)), pour autant qu’au moins une interface reste attachée à la machine virtuelle.
- Par défaut, la première interface réseau d’une machine virtuelle est définie comme l’interface réseau *principale*. Toutes les autres interfaces réseau de la machine virtuelle sont des interfaces réseau *secondaires*.
- Les serveurs DHCP Azure affectent une passerelle par défaut aux interfaces réseau principales, mais pas aux interfaces réseau secondaires. Étant donné qu’aucune passerelle par défaut n’est affectée aux interfaces réseau secondaires, par défaut, celles-ci ne peuvent pas communiquer avec des ressources situées hors de leur sous-réseau. Pour permettre aux interfaces réseau secondaires de communiquer avec les ressources hors de leur sous-réseau, consultez [Routage au sein du système d’exploitation d’une machine virtuelle avec plusieurs interfaces réseau](#routing-within-a-virtual-machine-operating-system-with-multiple-network-interfaces).
- Par défaut, tout le trafic sortant de la machine virtuelle est envoyé à l’adresse IP affectée à la configuration IP principale de l’interface réseau principale. Vous contrôlez l’adresse IP qui est utilisée pour le trafic sortant à l’intérieur du système d’exploitation de la machine virtuelle mais, par défaut, il transite par l’interface réseau principale.
- Auparavant, toutes les machines virtuelles du même groupe à haute disponibilité étaient requises pour une seule ou plusieurs interfaces réseau. Des machines virtuelles comportant un nombre quelconque d’interfaces réseau peuvent désormais exister dans le même groupe à haute disponibilité, pour autant que ce nombre soit pris en charge par la taille de la machine virtuelle. Cependant, vous ne pouvez ajouter une machine virtuelle à un groupe à haute disponibilité qu’au moment de la création de celui-ci. Pour en savoir plus sur les groupes à haute disponibilité, voir [Gérer la disponibilité des machines virtuelles dans Azure](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).
- Si des interfaces réseau attachées à la même machine virtuelle peuvent être connectées à différents sous-réseaux d’un réseau virtuel, les interfaces réseau doivent toutes être connectées au même réseau virtuel.
- Vous pouvez ajouter n’importe quelle adresse IP pour n’importe quelle configuration IP d’une interface réseau principale ou secondaire à un pool principal Azure Load Balancer. Auparavant, seule l’adresse IP principale de l’interface réseau principale pouvait être ajoutée à un pool principal. Pour en savoir plus sur les configurations et les adresses IP, voir [Ajouter, modifier ou supprimer des adresses IP](virtual-network-network-interface-addresses.md).
- La suppression d’une machine virtuelle n’a pas pour effet de supprimer les interfaces réseau qui y sont attachées. Lorsqu’une machine virtuelle est supprimée, les interfaces réseau sont détachées de la machine virtuelle. Vous pouvez attacher les interfaces réseau à différentes machines virtuelles, ou les supprimer de celles-ci.
- Si une interface réseau dispose d’une adresse IPv6 privée qui lui est assignée, vous pouvez l’attacher à une machine virtuelle lors de la création de cette dernière. Vous ne pouvez pas attacher d’interface réseau avec une adresse IPv6 assignée à une machine virtuelle une fois cette dernière créée. Si vous attachez une interface réseau avec une adresse IPv6 privée lors de la création d’une machine virtuelle, vous pouvez uniquement attacher cette interface réseau à la machine virtuelle, quel que soit le nombre d’interfaces réseau pris en charge par la taille de machine virtuelle. Consultez [Ajouter, modifier ou supprimer des adresses IP pour des cartes réseau Azure](virtual-network-network-interface-addresses.md) pour en savoir plus sur l’assignation d’adresses IP à des interfaces réseau.

## <a name="vm-create"></a>Ajouter des interfaces réseau existantes à une nouvelle machine virtuelle

Lorsque vous créez une machine virtuelle via le portail, celui-ci crée une interface réseau avec des paramètres par défaut, et l’attache à la machine virtuelle pour vous. Vous ne pouvez pas ajouter d’interfaces réseau existantes à une nouvelle machine virtuelle, ou créer une machine virtuelle avec plusieurs interfaces réseau via le portail Azure. Vous pouvez en revanche effectuer les deux opérations à l’aide de l’interface de ligne de commande ou de PowerShell. Vous pouvez ajouter à une machine virtuelle autant d’interfaces réseau que le permet la taille de la machine virtuelle que vous créez. Pour découvrir le nombre d’interfaces réseau prises en charge par chaque taille de machine virtuelle, lisez les articles sur les tailles des machines virtuelles [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Actuellement, les interfaces réseau que vous ajoutez à une machine virtuelle ne peuvent pas être attachées à une autre machine virtuelle. Pour en savoir plus sur la création, la modification ou la suppression d’interfaces réseau, consultez l’article [Gérer les interfaces réseau](virtual-network-network-interface.md#create-a-network-interface).

### <a name="routing-within-a-virtual-machine-operating-system-with-multiple-network-interfaces"></a>Routage au sein du système d’exploitation d’une machine virtuelle avec plusieurs interfaces réseau

Azure affecte une passerelle par défaut aux premières (principales) interfaces réseau associées à la machine virtuelle. Azure n’affecte pas de passerelle par défaut aux interfaces réseau additionnelles (secondaires) associées à la machine virtuelle. Par conséquent, vous ne pouvez pas communiquer avec les ressources hors du sous-réseau dans lequel se trouve, par défaut, une interface réseau secondaire. Toutefois, les interfaces réseau secondaires peuvent communiquer avec les ressources hors de leur sous-réseau. Ceci dit, les étapes à suivre pour permettre cette communication sont différentes d’un système d’exploitation à un autre.

### <a name="windows"></a>Windows

Effectuez les étapes suivantes depuis une invite de commandes Windows :

1. Exécutez la commande `route print`, qui retourne une sortie similaire à la sortie suivante d’une machine virtuelle à deux interfaces réseau :

    ```
    ===========================================================================
    Interface List
    3...00 0d 3a 10 92 ce ......Microsoft Hyper-V Network Adapter #3
    7...00 0d 3a 10 9b 2a ......Microsoft Hyper-V Network Adapter #4
    ===========================================================================
    ```
 
    Dans cet exemple, **Microsoft Hyper-V Network Adapter #4** (interface 7) est l’interface réseau secondaire à laquelle aucune passerelle par défaut n’est assignée.

2. Depuis une invite de commande, exécutez la commande `ipconfig` pour voir quelle adresse IP est assignée à l’interface réseau secondaire. Dans cet exemple, l’adresse 192.168.2.4 est assignée à l’interface 7. Aucune adresse de passerelle par défaut n’est retournée pour l’interface réseau secondaire.

3. Pour acheminer le trafic destiné aux adresses hors du sous-réseau de l’interface réseau secondaire vers la passerelle du sous-réseau, exécutez la commande suivante :

    ```
    route add -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5015 IF 7
    ```

    L’adresse de la passerelle du sous-réseau est la première adresse IP (celle qui finit par .1) dans la plage d’adresse définie pour le sous-réseau. Si vous ne voulez pas acheminer le trafic hors du sous-réseau, vous avez la possibilité d’ajouter des itinéraires individuels vers des destinations spécifiques. Par exemple, si vous voulez acheminer le trafic de l’interface réseau secondaire vers le réseau 192.168.3.0 uniquement, entrez la commande :

      ```
      route add -p 192.168.3.0 MASK 255.255.255.0 192.168.2.1 METRIC 5015 IF 7
      ```
  
4. Pour vérifier si la communication fonctionne avec une ressource du réseau 192.168.3.0, par exemple, entrez la commande suivante pour faire un test Ping de l’adresse 192.168.3.4 à l’aide de l’interface 7 (192.168.2.4) :

    ```
    ping 192.168.3.4 -S 192.168.2.4
    ```

    Vous devrez peut-être ouvrir le protocole ICMP via le pare-feu Windows de l’appareil sur lequel vous effectuez le test Ping. Exécutez cette commande :
  
      ```
      netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
      ```
  
5. Pour confirmer que l’itinéraire ajouté se trouve dans la table de routage, entrez la commande `route print`, qui retourne une sortie similaire à ce qui suit :

    ```
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4     15
              0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.4   5015
    ```

    L’itinéraire répertorié avec l’adresse *192.168.1.1* sous **Passerelle** représente l’itinéraire se trouvant là par défaut pour l’interface réseau principale. L’itinéraire avec l’adresse *192.168.2.1* sous **Passerelle** représente l’itinéraire que vous avez ajouté.

### <a name="linux"></a>Linux

Étant donné que le comportement par défaut utilise un routage d’hôte faible, nous recommandons de limiter le trafic d’interfaces réseau secondaires entre ressources à un seul sous-réseau. S’il vous faut des communications hors du sous-réseau des interfaces réseau secondaires, vous devez créer des règles de routage autorisant la machine virtuelle à envoyer et à recevoir du trafic via une interface réseau spécifique. Sinon, le trafic appartenant à eth1, par exemple, ne peut pas être traité correctement par l’itinéraire défini par défaut. Pour savoir comment configurer des règles de routage consultez [Configuration de plusieurs cartes d’interface réseau sous Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics).

> [!WARNING]
> Si une interface réseau dispose d’une adresse IPv6 privée qui lui est assignée, vous pouvez uniquement ajouter cette interface réseau à la machine virtuelle lors de la création de cette dernière. Vous ne pouvez pas associer plusieurs interfaces réseau supplémentaires à la machine virtuelle lorsque vous la créez, ou une fois qu’elle est créée, tant que l’adresse IPv6 est assignée à une interface réseau attachée à une machine virtuelle. Consultez [Ajouter, modifier ou supprimer des adresses IP pour des cartes réseau Azure](virtual-network-network-interface-addresses.md) pour en savoir plus sur l’assignation d’adresses IP à des interfaces réseau.

**Commandes**

|Outil|Commande|
|---|---|
|Interface de ligne de commande|[az vm create](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Ajouter une interface réseau existante à une machine virtuelle existante

Vous pouvez ajouter à une machine virtuelle autant d’interfaces réseau que le permet la taille de ladite machine virtuelle. Pour découvrir le nombre d’interfaces réseau prises en charge par chaque taille de machine virtuelle, lisez les articles sur les tailles des machines virtuelles [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). La machine virtuelle à laquelle vous souhaitez ajouter une interface réseau doit prendre en charge le nombre d’interfaces réseau que vous souhaitez ajouter et se trouver à l’état arrêté (libéré). Actuellement, les interfaces réseau que vous voulez ajouter ne peuvent pas être attachées à une autre machine virtuelle. Vous ne pouvez pas ajouter d’interface réseau à une machine virtuelle existante via le portail Azure. Pour ajouter des interfaces réseau à une machine virtuelle existante, vous devez utiliser l’interface de ligne de commande ou PowerShell. 

Azure affecte une passerelle par défaut aux premières (principales) interfaces réseau associées à la machine virtuelle. Azure n’affecte pas de passerelle par défaut aux interfaces réseau additionnelles (secondaires) associées à la machine virtuelle. Par conséquent, vous ne pouvez pas communiquer avec les ressources hors du sous-réseau dans lequel se trouve, par défaut, une interface réseau secondaire. Toutefois, les interfaces réseau secondaires peuvent communiquer avec des ressources hors de leur sous-réseau. S’il faut que vos interfaces réseau secondaires communiquent avec les ressources hors de leur sous-réseau, consultez [Routage au sein du système d’exploitation d’une machine virtuelle avec plusieurs interfaces réseau](#routing-within-a virtual-machine-operating-system-with-multiple-network-interfaces).

> [!WARNING]
> Si une interface réseau possède une adresse IPv6 privée qui lui est assignée, elle ne peut pas être ajoutée à une machine virtuelle existante. Vous pouvez uniquement ajouter une interface réseau avec une adresse IPv6 privée assignée à une machine virtuelle lorsque vous créez une machine virtuelle. Consultez [Ajouter, modifier ou supprimer des adresses IP pour des cartes réseau Azure](virtual-network-network-interface-addresses.md) pour en savoir plus sur l’assignation d’adresses IP à des interfaces réseau.

|Outil|Commande|
|---|---|
|Interface de ligne de commande|[az vm nic add](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add) (référence) ou [procédure détaillée](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (référence) ou [procédure détaillée](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a> Afficher les interfaces réseau d’une machine virtuelle

Vous pouvez afficher les interfaces réseau actuellement attachées à une machine virtuelle pour découvrir la configuration de chaque interface réseau et l’adresse IP qui lui est attribuée. 

1. Connectez-vous au [portail Azure](https://portal.azure.com) à l’aide d’un compte disposant des autorisations associées au rôle Propriétaire, Collaborateur ou Collaborateur de réseau pour votre abonnement. Pour en savoir plus sur l’assignation de rôles à des comptes, consultez [Rôles intégrés pour le contrôle d’accès en fonction du rôle Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Dans la zone qui contient le texte *Rechercher des ressources* en haut du portail Azure, saisissez *machines virtuelles*. Lorsque la mention **machines virtuelles** apparaît dans les résultats de recherche, cliquez dessus.
3. Dans le panneau **Machines virtuelles** qui s’affiche, cliquez sur le nom de la machine virtuelle pour laquelle vous souhaitez afficher les interfaces réseau.
4. Dans la section **PARAMÈTRES** du panneau Machine virtuelle qui s’affiche pour la machine virtuelle sélectionnée, cliquez sur **Mise en réseau**. Pour en savoir plus sur les paramètres d’interface réseau et comment les modifier, consultez l’article [Gérer les interfaces réseau](virtual-network-network-interface.md). Pour en savoir plus sur l’ajout, la modification ou la suppression d’adresses IP d’une interface réseau, consultez [Ajouter, modifier ou supprimer des adresses IP pour des cartes réseau Azure](virtual-network-network-interface-addresses.md).

**Commandes**

|Outil|Commande|
|---|---|
|Interface de ligne de commande|[az vm show](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a> Supprimer une interface réseau d’une machine virtuelle

La machine virtuelle dont vous souhaitez supprimer (ou dissocier) une interface réseau doit se trouver à l’état arrêté (désalloué) et au moins deux interfaces réseau doivent y être attachées. Vous pouvez supprimer n’importe quelle interface réseau, mais au moins une interface réseau doit toujours être attachée à la machine virtuelle. Si vous supprimez une interface réseau principale, Azure affecte l’attribut principal à l’interface réseau restée attachée à la machine virtuelle le plus longtemps. 

1. Connectez-vous au [portail Azure](https://portal.azure.com) à l’aide d’un compte disposant des autorisations associées au rôle Propriétaire, Collaborateur ou Collaborateur de réseau pour votre abonnement. Pour en savoir plus sur l’assignation de rôles à des comptes, consultez [Rôles intégrés pour le contrôle d’accès en fonction du rôle Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Dans la zone qui contient le texte *Rechercher des ressources* en haut du portail Azure, saisissez *machines virtuelles*. Lorsque la mention **machines virtuelles** apparaît dans les résultats de recherche, cliquez dessus.
3. Dans le panneau **Machines virtuelles** qui s’affiche, cliquez sur le nom de la machine virtuelle pour laquelle vous souhaitez supprimer une interface réseau.
4. Dans la section **PARAMÈTRES** du panneau Machine virtuelle qui s’affiche pour la machine virtuelle sélectionnée, cliquez sur **Mise en réseau**. Pour en savoir plus sur les paramètres d’interface réseau et comment les modifier, consultez l’article [Gérer les interfaces réseau](virtual-network-network-interface.md). Pour en savoir plus sur l’ajout, la modification ou la suppression d’adresses IP d’une interface réseau, consultez [Ajouter, modifier ou supprimer des adresses IP pour des cartes réseau Azure](virtual-network-network-interface-addresses.md).
5. Cliquez sur **Détacher interface réseau**.
6. Sélectionnez l’interface réseau que vous souhaitez détacher depuis la liste déroulante, puis cliquez sur **OK**.

**Commandes**

|Outil|Commande|
|---|---|
|Interface de ligne de commande|[az vm nic remove](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#remove) (référence) ou [procédure détaillée](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Remove-AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (référence) ou [procédure détaillée](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>Étapes suivantes
Pour créer une machine virtuelle avec plusieurs interfaces réseau ou adresses IP, lisez les articles suivants :

**Commandes**

|Tâche|Outil|
|---|---|
|Créer une machine virtuelle avec plusieurs cartes d’interface réseau|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Créer une machine virtuelle à carte réseau unique avec plusieurs adresses IPv4|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Créer une machine virtuelle à carte réseau unique avec une adresse IPv6 privée (derrière Azure Load Balancer)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [modèle Azure Resource Manager](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
